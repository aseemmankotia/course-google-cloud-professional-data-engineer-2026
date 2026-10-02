## Chapter 7 Cheat Sheet: Bigtable, Spanner, Cloud SQL, Cloud Storage

### Bigtable Row-Key Design
| Do | Don't |
|---|---|
| Hash/salt or reverse high-cardinality prefix | Leading monotonic ID or raw ascending timestamp |
| Reverse timestamp as suffix for recent-first scans | Sequential auto-increment as leading key |
| Tall-narrow tables, single index = row key | Rely on secondary indexes (none exist) |

**Limits:** cell ≤100MB, row ≤256MB. Single-cluster = strong consistency; multi-cluster = eventual consistency across clusters.
**Best fit:** IoT, time-series, high-throughput key-value, no SQL, no multi-row transactions.

### Spanner vs Cloud SQL vs Bigtable
| Feature | Cloud SQL | Spanner | Bigtable |
|---|---|---|---|
| Model | Relational (MySQL/Postgres/SQL Server) | Relational, SQL | NoSQL wide-column |
| Scale | Vertical, single region, ≤64TB | Horizontal, global, petabytes | Horizontal, petabytes, high QPS |
| Transactions | ACID (single instance) | ACID multi-row/table, external strong consistency | Atomic per-row only |
| SLA | Regional HA via failover | 99.99% regional / 99.999% multi-region | Depends on cluster replication |
| Scaling unit | Instance tier | Processing Units (1000 PU = 1 node, min 100 PU) | Nodes (add for throughput) |
| Use when | Traditional app DB, moderate scale | Global txn workload, need SQL + massive scale | High-throughput ingestion, simple key lookups |

### Cloud Storage Classes
| Class | Access pattern | Min duration | Relative cost |
|---|---|---|---|
| Standard | Frequent / hot | None | Highest storage cost |
| Nearline | ~1x/month | 30 days | Lower storage, retrieval fee |
| Coldline | ~1x/quarter | 90 days | Lower still |
| Archive | <1x/year | 365 days | Lowest storage, highest retrieval fee |

**Lifecycle rules:** each action (SetStorageClass, Delete) needs its own explicit rule + condition (Age, CreatedBefore, etc.). No implicit deletion or class reversion.

### Decision Shortcuts
- "Global + strong consistency + SQL" → **Spanner**
- "Regional relational app, simple scale" → **Cloud SQL**
- "Millions of writes/sec, key-based lookup, no SQL" → **Bigtable**
- "Rarely accessed archival data, compliance retention" → **Coldline/Archive + lifecycle rule**
- Hotspotting fix → salt/hash/reverse the row key prefix, never lead with sequential values
