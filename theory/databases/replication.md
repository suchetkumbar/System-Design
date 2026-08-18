# Replication

## Summary

Replication is the practice of copying data across multiple nodes to improve availability, durability, and read performance.

- **Leader-Follower**: One primary node accepts writes; replicas serve reads
- **Multi-Leader**: Multiple nodes accept writes, conflicts must be resolved
- **Leaderless**: All nodes are equal; quorum-based consistency
- **Replication Lag**: Followers lag behind the leader temporarily
- **Eventual Consistency**: Followers eventually catch up to the leader

Replication is essential for reliable, scalable systems but introduces complexity.

## Why It Matters

Replication enables:
- **High Availability**: System survives node failures
- **Disaster Recovery**: Data preserved across geographic regions
- **Read Scaling**: Distribute read load across replicas
- **Geographic Distribution**: Serve data closer to users
- **Durability**: Multiple copies reduce data loss risk

Without replication, a single node failure means data loss or downtime.

## How It Works

### Leader-Follower Replication

**Architecture:**
```
Writes → Leader ─→ Replication Log ─→ Followers
         ↓                              ↓
       Commit                       Apply & Serve Reads
```

**Process:**
1. Client writes to leader
2. Leader writes to local log
3. Leader sends log to followers
4. Followers apply changes
5. Followers acknowledge
6. Client sees success

**Read Behavior:**
- Writes: Must go to leader
- Reads: Can go to leader or followers
- Followers may be behind (replication lag)

**Examples:** PostgreSQL, MySQL, MongoDB, Redis

**Failure Scenarios:**

| Failure | Impact | Recovery |
|---------|--------|----------|
| Follower fails | Read capacity reduced | Restart node, catch up from log |
| Leader fails | Writes unavailable | Promote highest follower, update config |
| Network partition | Lag increases | Wait for reconnection or failover |

### Multi-Leader Replication

**Architecture:**
```
Write A → Leader 1 ──┐
                     ├→ Replication ←→ Leader 2 ← Write B
Write A → Leader 1 ──┘                  ↓
                                    Followers
```

**Characteristics:**
- Multiple leaders can accept writes
- Leaders replicate to each other
- All leaders replicate to followers
- Write conflicts possible

**Conflict Resolution:**

1. **Last-Write-Wins (LWW)**
   - Discard writes with older timestamps
   - Risk: Data loss silently
   - Use: When acceptable

2. **Multi-Value**
   - Keep conflicting versions
   - Application resolves later
   - Use: Collaboration tools

3. **Custom Logic**
   - Application-specific merge
   - Example: CRDTs (Conflict-free Replicated Data Types)
   - Use: Complex semantics

**Examples:** Cassandra, DynamoDB, Riak

**When to Use:**
- Geographic distribution (write locally, replicate globally)
- High availability with active-active setup
- Collaborative systems (documents, notes)

**Challenges:**
- Conflict resolution is hard
- Network partitions are more complex
- Operational overhead increases

### Leaderless Replication

**Architecture:**
```
Write → Node 1
        ↓
        Node 2 (Quorum-based)
        ↓
      Node 3

Read Repair + Anti-entropy
```

**Characteristics:**
- All nodes are equal
- No single point of failure
- Quorum-based read/write
- Anti-entropy background process

**Quorum Consistency:**

```
W = write replicas
R = read replicas
N = total replicas

If W + R > N → Strong consistency
If W + R ≤ N → Weak consistency
```

**Examples:**
- W=N, R=1: Fast reads, slow writes
- W=1, R=N: Fast writes, slow reads
- W=N/2+1, R=N/2+1: Balanced

**Examples:** DynamoDB, Riak, Cassandra (quorum mode), Dynamo

**Advantages:**
- No failover needed
- Better availability
- Write tolerance: survive (N-W) failures

**Disadvantages:**
- Complex read/write logic
- Read repair overhead
- Inconsistent views possible

## Replication Lag

### Definition

The time delay between a write to the leader and its visibility on followers.

**Timeline:**
```
t=0:     Client writes to leader
t=10ms:  Leader applies write
t=20ms:  Follower 1 receives
t=30ms:  Follower 1 applies
t=50ms:  Follower 2 receives
t=60ms:  Follower 2 applies

Replication Lag = 60ms - 10ms = 50ms
```

### Consequences of Replication Lag

**Problem 1: Stale Reads**
```
1. User writes new password
2. User immediately reads from follower (hasn't updated yet)
3. User sees old password
```

**Problem 2: Non-Monotonic Reads**
```
1. User reads post count = 100 from Follower A
2. Follower A lags, hasn't received 10 new posts yet
3. User reads post count = 95 from Follower B
4. Count goes backwards (non-monotonic)
```

**Problem 3: Causality Violations**
```
1. User posts comment
2. Other user reads from lagging follower
3. Other user doesn't see the comment yet
4. Other user's read violates causality
```

### Mitigation Strategies

| Strategy | How It Works | Trade-Off |
|----------|-------------|-----------|
| **Read from Leader** | Write → Leader; Read → Leader | Reduces scalability |
| **Session Consistency** | User always reads from same follower | Sticky routing complexity |
| **Timestamp-Based** | Client includes write timestamp, waits on read | Latency increase |
| **Quorum Reads** | Read from multiple replicas, use majority | Slower reads |

## Replication Methods

### Statement-Based Replication

**Approach:** Send SQL statements to followers

```
Leader: INSERT INTO users VALUES (1, 'Alice')
Send to Followers: INSERT INTO users VALUES (1, 'Alice')
```

**Advantages:**
- Compact
- Easy to understand

**Disadvantages:**
- Non-deterministic functions (NOW(), RAND())
- Autoincrement dependencies
- Side effects not replicated
- Global state changes cause divergence

**Use:** Rare, mostly historical

### Write-Ahead Log (WAL) Shipping

**Approach:** Send low-level log records to followers

```
Leader applies change → Log entry written → Sent to followers
```

**Advantages:**
- Exact byte-for-byte replication
- Preserves all details

**Disadvantages:**
- Coupled to storage format
- Followers must handle same storage engine
- Difficult to upgrade

**Use:** PostgreSQL, MySQL with binary log

### Logical Log Replication

**Approach:** Send high-level change description to followers

```
{
  "table": "users",
  "operation": "INSERT",
  "values": {"id": 1, "name": "Alice"}
}
```

**Advantages:**
- Decoupled from storage
- Followers can use different engines
- Easier to understand
- Better for multi-version support

**Disadvantages:**
- Slightly larger overhead
- More parsing

**Use:** PostgreSQL logical decoding, MySQL row-based binlog, Cassandra

## Trade-Offs

### Consistency vs Availability

| Strategy | Consistency | Availability | Latency |
|----------|------------|--------------|---------|
| **Strong Read** | Immediate | Lower (all nodes) | Higher |
| **Eventual** | Delayed | High (any node ok) | Lower |
| **Causal** | Logical order preserved | Medium | Medium |
| **Quorum** | Hybrid (tunable) | Tunable | Tunable |

### Write Complexity vs Read Performance

- **Many small followers**: Easy reads, expensive writes
- **Few large followers**: Expensive reads, easier writes
- **Balanced**: Trade-off at middle ground

### Replication Lag vs Write Latency

- Synchronous: Wait for replicas (slow, safe)
- Asynchronous: Ack immediately (fast, risky)
- Semi-synchronous: Ack after at least one replica (balanced)

### Consistency Model vs Partition Tolerance

(See CAP Theorem for details)

## Failure Modes

### Replication Lag Increases

**Symptoms:**
- Followers fall behind
- Reads return stale data
- Monitoring shows lag > threshold

**Causes:**
- Network congestion
- Follower overloaded
- Large transaction on leader

**Recovery:**
- Wait (lag catches up)
- Add resources to follower
- Reduce write load temporarily

### Replication Breaks (Network Partition)

**Scenarios:**

1. **Temporary Partition:**
   - Leader and followers can't communicate
   - Followers stop receiving updates
   - Lag increases indefinitely
   - Recovery: Network heals, lag catches up

2. **Permanent Partition:**
   - Network can't heal
   - Leader and followers diverge
   - Data loss or inconsistency on recovery
   - Recovery: Manual intervention, reconciliation

**Handling:**
- Alert on lag threshold
- Monitor partition events
- Plan for failover
- Use transactions carefully

### Split Brain (Multi-Leader)

**Scenario:**
```
Network partition splits two leaders
Both accept writes independently
Data diverges
Network heals
Conflict resolution needed
```

**Prevention:**
- Use consensus (Raft, Paxos)
- Quorum-based decisions
- Avoid multi-leader when possible

## Interview Notes

### Questions to Ask

1. **Availability Requirements**
   - "How many 9's of availability?"
   - "Can we tolerate brief downtime?"
   - "Do all reads need fresh data?"

2. **Data Loss Tolerance**
   - "Is data loss acceptable?"
   - "What's the RPO (Recovery Point Objective)?"
   - "How many replicas do we need?"

3. **Geographic Distribution**
   - "Do users span multiple regions?"
   - "Are writes geographically distributed?"
   - "What's acceptable replication lag?"

4. **Consistency Model**
   - "Do we need strong or eventual consistency?"
   - "Can we tolerate stale reads?"
   - "What about causality violations?"

### Strategy Selection

**High Availability, Strong Consistency:**
- Synchronous replication
- Quorum reads/writes
- Trade: Slower writes
- Use: Financial systems, critical data

**High Availability, Eventual Consistency:**
- Asynchronous replication
- Read from any node
- Trade: Potential stale reads
- Use: Social media, caching

**Geographic Distribution:**
- Multi-leader or leaderless
- Conflict resolution strategy
- Trade: Conflict complexity
- Use: Global services

## Replication Architecture Patterns

### Read-Heavy System
```
Write → Leader ─┬→ Follower 1 ─→ Serve Reads
               ├→ Follower 2 ─→ Serve Reads
               └→ Follower 3 ─→ Serve Reads

Benefit: Scale reads
Cost: Replication lag, consistency
```

### Write-Heavy System
```
Write 1 → Leader 1 ─┬→ Followers
Write 2 → Leader 2 ─┤
Write 3 → Leader 3 ─┘

Benefit: Distribute writes
Cost: Conflict resolution
```

### Critical Data with Backups
```
Critical Data → Leader ─→ Synchronous Replica
             └─→ Async Replicas (backup)

Benefit: Safety with performance
Cost: Complexity
```

## Production Considerations

### Monitoring

- **Replication Lag**: Alert if > threshold (e.g., 1 second)
- **Replication Errors**: Alert on any errors
- **Replica Availability**: Count unavailable replicas
- **Write Throughput**: Monitor leader write rate
- **Read Distribution**: Verify reads spread across replicas

### Failover Strategy

**Automatic Failover:**
- Pros: Fast recovery, no manual intervention
- Cons: Risk of split brain, data loss

**Manual Failover:**
- Pros: Control, no accidental promotions
- Cons: Downtime, human error

**Hybrid:**
- Automatic detection and alerts
- Manual promotion decision
- Recommended: Balance safety and availability

### Backup Strategy

- Replicas are **not** backups
- Replication is **not** backup
- Need separate backup system
- Point-in-time recovery capability
- Test restore procedure regularly

## Related Topics

- [Consistency Models](../distributed-systems/consistency-models.md)
- [CAP Theorem and PACELC](../distributed-systems/cap-theorem-pacelc.md)
- [SQL vs NoSQL Trade-Offs](sql-vs-nosql-tradeoffs.md)
- Sharding and Partitioning (coming soon)
- Distributed Transactions (coming soon)
