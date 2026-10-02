# Chapter 4 Cheat Sheet: Batch ETL, Dataproc, and CDC

## Dataproc Cluster Types
| Type | Use Case | Cost Behavior |
|---|---|---|
| Ephemeral (Workflow Template) | Scheduled/one-off batch jobs | Billed only during job runtime; no idle cost |
| Persistent/long-running | Shared interactive notebooks, ad hoc queries | Billed continuously; manage with autoscaling/idle deletion |
| Single Node | Dev/test only | Not for production HA |
| High Availability (3 masters) | Mission-critical persistent clusters | Higher cost, resilient to master failure |

## Dataproc Cost/Perf Levers
- Billing: per-second, 1-minute minimum.
- Secondary workers: preemptible/Spot VMs for cost savings; primary workers must stay standard.
- Enable **graceful decommissioning** so tasks finish before a preemptible secondary worker is reclaimed.
- Autoscaling **cooldown period** default = 2 min (prevents scaling thrash).
- **Graceful decommission timeout**: default 0s if unset, max 1 day (86400s).
- Use custom/pre-baked images to cut cluster startup time (~90s baseline).
- Prefer ephemeral clusters via Workflow Templates/Composer for recurring batch ETL — biggest cost lever tested.

## Datastream (CDC)
| Source | CDC Mechanism | Prerequisite |
|---|---|---|
| Oracle | Log-based (LogMiner/XStream) reads redo logs | Supplemental logging enabled |
| MySQL | Binlog-based | binlog_format=ROW |
| PostgreSQL / Cloud SQL / AlloyDB | Logical replication (WAL) | wal_level=logical, replication slot, pgoutput plugin |
| SQL Server | Log-based | CDC enabled on source DB |

- Destinations: **BigQuery** (direct write, near real-time analytics) or **Cloud Storage** (Avro/JSON, staging/replay).
- Stream = **backfill** (historical snapshot) + **CDC** (ongoing changes).
- **Schema drift**: new/dropped columns auto-detected and propagated to BigQuery; some DDL (type changes) may need manual handling — validate.
- Private connectivity: VPC peering, Private Service Connect, or reverse SSH tunnel for on-prem/private sources.

## Idempotency Patterns (Retry-Safe Pipelines)
- **Staging table + MERGE** keyed on business/primary key = standard idempotent upsert pattern.
- Avoid blind append-on-retry; avoid full truncate-reload unless source is a fixed-point snapshot.
- For streaming inserts, dedupe using a stable `insertId`/idempotency key.
- Design each pipeline stage so re-running with the same input produces the same output state.

## Service Selection Quick Map
| Need | Service |
|---|---|
| Existing Spark/Hadoop code, custom libraries | Dataproc |
| DB change replication only, no transform | Datastream |
| Serverless unified batch+stream ETL, autoscaling, no ops | Dataflow |
| GUI-based batch pipeline authoring | Cloud Data Fusion |

## Common Exam Traps
- Choosing Dataflow/Data Fusion for pure CDC replication instead of Datastream.
- Making primary Dataproc workers preemptible/Spot (unsupported/unsafe — only secondary workers should be preemptible).
- Confusing cooldown period (scaling frequency) with graceful decommission timeout (worker removal delay).
- Designing pipelines with append-only retries instead of MERGE-based idempotent upserts.
