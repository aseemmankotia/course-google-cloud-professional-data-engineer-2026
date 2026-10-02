## BigQuery Storage Optimization — Cheat Sheet

### Partitioning Types
| Type | Column Requirement | Granularity | Notes |
|---|---|---|---|
| Time-unit | DATE/TIMESTAMP/DATETIME | HOUR/DAY/MONTH/YEAR | Most common; supports partition expiration |
| Ingestion-time | none (auto pseudo-column) | DAY (default) | Uses `_PARTITIONTIME`/`_PARTITIONDATE` |
| Integer-range | INTEGER column | start/end/interval | Good for numeric IDs |

**Max partitions per table: 4,000**

### Clustering
- Max **4 columns**, order = priority (most selective/filtered first)
- Works within partitions (or on unpartitioned tables)
- Best for high-cardinality columns unsuitable for partitioning

### Partitioning vs Clustering
| | Partitioning | Clustering |
|---|---|---|
| Pruning level | Metadata (skip whole segments) | Block-level sort |
| Max cols | 1 | 4 |
| Cardinality fit | Low/medium | High |
| Cost visibility | Bytes scanned shown before running | Estimated post-pruning |

### Materialized Views vs Scheduled Queries
| | Materialized View | Scheduled Query |
|---|---|---|
| Refresh | Automatic, incremental | Manual schedule (cron), full run |
| Query rewrite | Yes (optimizer auto-uses it) | No |
| SQL flexibility | Limited (aggregations, some joins) | Arbitrary SQL/DDL |
| Use case | Real-time dashboards | Batch ETL/transform jobs |

### Pricing Models
| Model | Billing Basis | Best For |
|---|---|---|
| On-demand | ~$6.25/TiB scanned (1 TiB/mo free) | Unpredictable/bursty workloads |
| Editions/Flat-rate (slots) | Per slot-hour reservation | Steady, high-volume workloads |

### Storage Pricing
| Tier | Trigger | Approx. Rate |
|---|---|---|
| Active | Modified within 90 days | ~$0.02/GB/mo |
| Long-term | Untouched 90+ days (automatic) | ~$0.01/GB/mo |

### Time Travel & Snapshots
- Default window: **7 days**; configurable **2–7 days**
- Table snapshots: point-in-time copy, billed for unique data only
- Lowering time travel window reduces storage cost for high-churn tables

### Cost Optimization Checklist
1. Partition by the most common filter column (usually date)
2. Cluster on next 1–4 most selective filter/join columns
3. Set partition expiration for stale data
4. Use materialized views for repeated aggregation queries
5. Match pricing model to workload predictability
6. Reduce time travel window if churn is high and recovery needs are low
