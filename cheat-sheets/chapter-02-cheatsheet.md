## Chapter 2 Cheat Sheet: Transformation Tools & Governance

### Tool Selection Matrix
| Need | Tool | Why |
|---|---|---|
| Lift-and-shift existing Hadoop/Spark | **Dataproc** | Managed clusters, unmodified code, ephemeral clusters recommended |
| New unified batch+stream, no ops | **Dataflow** | Serverless Apache Beam runner, autoscaling |
| No-code visual ETL for non-devs | **Data Fusion** | CDAP-based drag-and-drop studio, runs on Dataproc backend |
| SQL-based ELT inside BigQuery, version control | **Dataform** | SQLX + Git + dependency DAG + assertions, no separate compute |
| Governance, lineage, catalog | **Dataplex** | Lakes → Zones → Assets, auto-lineage, unified IAM policy |

### Apache Beam Model
- **Pipeline**: the full job graph
- **PCollection**: immutable distributed dataset (bounded=batch, unbounded=stream)
- **PTransform**: operation (ParDo, GroupByKey, Combine, Flatten)
- **Windowing**: Fixed (tumbling, non-overlapping), Sliding (overlapping, size+period), Session (gap-based), Global (default, whole dataset)
- **Triggers**: control *when* results are emitted per window (e.g., early/late firing) relative to the watermark

### Dataproc Essentials
- Runs Hadoop, Spark, Hive, Pig, Presto on managed VM clusters
- Ephemeral (job-scoped) clusters = best practice, cheaper than long-running clusters
- Autoscaling policy: min/max secondary workers, scales on YARN pending memory
- Dataproc Serverless: run Spark batches/interactive without managing clusters at all

### Data Fusion Editions
| Edition | Use case |
|---|---|
| Basic | Simple, no versioning |
| Enterprise | Production, HA, versioning |
| Developer | Dev/test, lower cost |

### Dataform Key Concepts
- `.sqlx` files define tables/views/incremental tables
- `ref()` function builds the dependency graph automatically
- Assertions = built-in data quality tests (e.g., uniqueness, not-null)
- Now natively integrated as **Dataform in BigQuery** (workspaces, workflows, scheduling)

### Dataplex Governance Hierarchy
```
Lake (business domain)
└── Zone (Raw or Curated)
    └── Asset (BigQuery dataset / GCS bucket)
```
- Auto-discovers schema, generates lineage from Dataflow/Dataproc/BigQuery jobs
- Enforces IAM + data quality rules centrally across zones

### Data Mesh on GCP
1. Domain-oriented ownership → mapped to Dataplex Lakes
2. Data as a product → curated Zones with quality SLAs
3. Self-serve platform → BigQuery/Dataplex/Dataform tooling
4. Federated governance → Dataplex policies + org-level IAM

### Common Exam Traps
- Choosing Dataflow for a straight Hadoop/Spark migration (should be Dataproc)
- Choosing Data Fusion when the team already codes in SQL and wants Git-based CI (should be Dataform)
- Assuming Data Catalog alone equals full governance — Dataplex adds lineage + zone-based enforcement on top
- Confusing sliding windows (overlapping) with fixed windows (non-overlapping)
