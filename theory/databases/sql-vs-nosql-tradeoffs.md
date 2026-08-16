# SQL vs NoSQL Trade-Offs

## Summary

SQL (relational) and NoSQL (non-relational) databases represent fundamentally different approaches to data storage and retrieval.

- **SQL**: ACID-compliant, normalized schemas, powerful queries, strong consistency
- **NoSQL**: Flexible schemas, high availability, horizontal scaling, eventual consistency

The choice depends on access patterns, consistency requirements, scale, and team expertise.

## Why It Matters

Most systems require persistent data storage. The database choice affects:
- Query performance and flexibility
- Consistency guarantees
- Scaling complexity
- Operational overhead
- Cost and latency
- Team productivity

Choosing the wrong database early is expensive to fix.

## How It Works

### SQL (Relational Databases)

**Structure:**
- Fixed schema with tables, rows, and columns
- Relationships defined through foreign keys
- Strong typing for each column

**Querying:**
- SQL language for flexible querying
- JOINs to combine data across tables
- Transactions for multi-step operations
- ACID guarantees

**Examples:** PostgreSQL, MySQL, MariaDB, Oracle, SQL Server

**Typical Architecture:**
```
Client → SQL Query → Query Optimizer → Storage Engine → Disk
         ↓ (Result Set)
```

### NoSQL (Non-Relational Databases)

**Structure:**
- Flexible or schema-less design
- Document, key-value, column-family, or graph models
- No built-in relationships

**Querying:**
- Database-specific APIs (not standard SQL)
- Limited JOINs (usually done in application)
- Single document/record access patterns
- Eventual consistency

**Examples:** MongoDB, DynamoDB, Cassandra, Redis, Couchbase, Neo4j

**Typical Architecture:**
```
Client → API → Distributed Nodes → Replication → Disk/Memory
         ↓ (Response)
```

## Trade-Offs

### Consistency & Reliability

| Aspect | SQL | NoSQL |
|--------|-----|-------|
| **Consistency Model** | Strong (ACID) | Eventual (BASE) |
| **Transactions** | Multi-row transactions | Single document/record |
| **Data Integrity** | Enforced by schema | Application responsibility |
| **Recovery** | Predictable | May lose in-flight writes |

**SQL Wins:**
- Bank transfers, payments, financial transactions
- Systems requiring strong consistency
- Multi-table updates

**NoSQL Wins:**
- Real-time feeds, analytics, caching
- Systems tolerating temporary inconsistency
- High availability over instant consistency

### Scalability

| Aspect | SQL | NoSQL |
|--------|-----|-------|
| **Horizontal Scaling** | Difficult (sharding required) | Native (partition-key design) |
| **Vertical Scaling** | Easier (bigger machines) | Also possible |
| **Read Scaling** | Read replicas | Replication to many nodes |
| **Write Scaling** | Limited by leader | Better through sharding |

**SQL Limitations:**
- Scaling requires application-level sharding
- Joins across shards are expensive
- Distributed transactions are complex

**NoSQL Advantages:**
- Built for distributed systems
- Consistent hashing, partitioning
- Write distribution across nodes

### Query Flexibility

| Aspect | SQL | NoSQL |
|--------|-----|-------|
| **Ad-hoc Queries** | Easy (any combination of columns) | Difficult (schema-dependent) |
| **Aggregations** | Powerful (GROUP BY, window functions) | Limited or requires application logic |
| **Joins** | Native and optimized | Simulated in application |
| **Indexes** | Flexible, added later | Must plan ahead |

**SQL Wins:**
- Analytics and reporting
- Business intelligence
- Exploratory queries

**NoSQL Challenge:**
- Schema must match access patterns
- Changing queries may require migration

### Operational Complexity

| Aspect | SQL | NoSQL |
|--------|-----|-------|
| **Operational Maturity** | Decades of best practices | Younger, evolving patterns |
| **Backup & Recovery** | Well-established | Varies by system |
| **Monitoring** | Standard metrics | Distributed system complexity |
| **Tuning** | Indexes, query optimization | Replication, partitioning, consistency |

**SQL:**
- More predictable behavior
- Smaller operational surface
- Easier debugging

**NoSQL:**
- More distributed complexity
- Harder to troubleshoot
- Steeper learning curve

### Data Model Flexibility

| Aspect | SQL | NoSQL |
|--------|-----|-------|
| **Schema Changes** | Migrations required | Can add fields freely |
| **Nested Data** | Normalized (multiple tables) | Native (documents) |
| **Flexibility** | Rigid | Loose |
| **Evolution** | Expensive | Cheap |

**SQL Challenge:**
- Adding columns requires migrations
- Denormalization for performance is manual

**NoSQL Advantage:**
- Add fields without migration
- Store complex nested structures
- Easy iterative schema evolution

### Cost

| Aspect | SQL | NoSQL |
|--------|-----|-------|
| **Hardware** | Fewer large machines | Many smaller machines |
| **Licensing** | Often commercial (expensive) | Usually open-source (free) |
| **Operations** | Lower (simpler) | Higher (distributed) |
| **Cloud Cost** | Predictable | Depends on usage patterns |

## Failure Modes

### SQL Failure Modes

1. **Scalability Wall**: Sharding complexity, join performance degrades
2. **Deadlocks**: Concurrent transactions may deadlock
3. **Long Locks**: Large transactions block reads
4. **Replication Lag**: Read replicas may serve stale data
5. **Split Brain**: Primary failure can cause unavailability
6. **Storage Limits**: Vertical scaling has physical limits

### NoSQL Failure Modes

1. **Eventual Consistency Surprise**: Reads return stale data
2. **Lack of Transactions**: No multi-document ACID guarantees
3. **Hot Key Problem**: Uneven load distribution
4. **Shard Imbalance**: Partitioning can cause hot shards
5. **Lost Writes**: Async replication may lose data
6. **Query Limitations**: Unforeseen access patterns require refactoring

## Interview Notes

### Questions to Ask

When choosing between SQL and NoSQL, clarify:

1. **Consistency Requirements**
   - "Do we need strong consistency for all reads?"
   - "Is eventual consistency acceptable?"
   - "What is the tolerance for stale data?"

2. **Scale Expectations**
   - "What's the expected write throughput?"
   - "How many shards will we need?"
   - "What's the data size in 5 years?"

3. **Query Patterns**
   - "What are the main access patterns?"
   - "Do we need complex joins?"
   - "Will queries change frequently?"

4. **Availability**
   - "Is uptime more important than consistency?"
   - "Can we tolerate partial failures?"
   - "What's the RTO/RPO requirement?"

### Hybrid Approaches

**Polyglot Persistence**: Use both in the same system

- SQL for transactional data (orders, payments)
- NoSQL for caching, feeds, analytics
- Example: PostgreSQL + Redis + Cassandra

**When to Use:**
- Different access patterns require different databases
- Specialized tools for specialized jobs
- Cost-benefit analysis favors multiple systems

### Common Patterns

**SQL-first:**
- Mature applications with complex relationships
- Strong consistency requirements
- Moderate scale

**NoSQL-first:**
- High throughput, distributed systems
- Simple access patterns
- Rapid iteration on schema

**Pragmatic Mixed:**
- SQL for critical data
- NoSQL for high-volume, low-consistency data
- Replication between systems as needed

## Decision Tree

```
START
├─ Do we need multi-row ACID transactions?
│  ├─ YES → SQL
│  └─ NO → Continue
├─ Will we query by many different column combinations?
│  ├─ YES → SQL
│  └─ NO → Continue
├─ Do we expect write throughput > 10k/sec?
│  ├─ YES → NoSQL
│  └─ NO → Continue
├─ Is strong consistency required?
│  ├─ YES → SQL
│  └─ NO → NoSQL (probably)
└─ Does the schema change frequently?
   ├─ YES → NoSQL
   └─ NO → SQL
```

## Production Considerations

### SQL Deployment

- Backup strategy (point-in-time recovery)
- Replication setup (leader-follower or multi-leader)
- Read replica scaling
- Monitoring slow queries
- Index maintenance
- Connection pooling

### NoSQL Deployment

- Replication factor (usually 3)
- Consistency level configuration
- Partition key strategy
- Hotspot monitoring
- Compaction and maintenance windows
- Quorum consistency for critical reads

## Related Topics

- [Database Indexing and Query Patterns](database-indexing-query-patterns.md)
- [Consistency Models](../distributed-systems/consistency-models.md)
- [CAP Theorem and PACELC](../distributed-systems/cap-theorem-pacelc.md)
- [Caching Strategies](../fundamentals/caching-strategies.md)
- Replication (coming soon)
- Database Sharding (coming soon)
