## Chapter 4: Batch ETL, Dataproc, and Change Data Capture — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Datastream CDC mechanism for Oracle sources? | Log-based capture (LogMiner/XStream) reading redo logs — no source polling, minimal load. Exam tip: Datastream uses log-based CDC across Oracle/MySQL/PostgreSQL, not query polling. |
| 2 | MySQL prerequisite for Datastream CDC? | binlog_format=ROW must be enabled on the source. Exam tip: without ROW-format binlog, Datastream CDC cannot capture row-level changes. |
| 3 | PostgreSQL prerequisite for Datastream CDC? | wal_level=logical, a replication slot, and the pgoutput plugin. Exam tip: applies to Cloud SQL, AlloyDB, and self-managed PostgreSQL sources alike. |
| 4 | Datastream supported destinations? | BigQuery (direct streaming write) and Cloud Storage (Avro/JSON change files). Exam tip: choose BigQuery for near-real-time analytics, Cloud Storage for staging/replay pipelines. |
| 5 | When to use an ephemeral Dataproc cluster? | Per-job clusters spun up via Workflow Templates/Composer, deleted after the job finishes — no idle-cluster cost. Exam tip: recommended pattern for scheduled batch ETL jobs. |
| 6 | When to use a persistent Dataproc cluster? | Long-running cluster shared by multiple users for interactive notebooks (via Component Gateway) or ad hoc queries. Exam tip: costs accrue while idle — mitigate with autoscaling or idle-deletion timeout. |
| 7 | Dataproc billing granularity? | Billed per second with a 1-minute minimum charge per cluster. Exam tip: ephemeral clusters minimize waste because billing stops the moment the cluster is deleted. |
| 8 | Dataproc secondary worker options? | Preemptible or Spot VMs (non-persistent, cheaper, reclaimable anytime) added alongside standard primary workers. Exam tip: enable graceful decommissioning so in-flight tasks finish before a secondary worker is reclaimed. |
| 9 | Idempotent batch load pattern into BigQuery? | Load into a staging table, then MERGE into the target keyed on a business/primary key — reruns produce an identical end state, no duplicates. Exam tip: avoid blind append-and-retry designs. |
| 10 | Dataproc autoscaling cooldown period default? | 2 minutes — minimum wait between scaling actions to prevent thrashing. Exam tip: distinct from graceful decommission timeout, which governs worker removal, not scaling cadence. |
| 11 | How does Datastream handle schema drift? | It automatically detects new/dropped columns from supported sources and propagates changes to the BigQuery destination; not every DDL type (e.g., column type changes) is fully automatic — validate downstream. Exam tip: heavily tested — know both the automation and its limits. |
| 12 | Dataproc vs Datastream vs Dataflow — which for which scenario? | Dataproc = existing Spark/Hadoop batch code needing custom libraries; Datastream = database CDC replication only (no transforms); Dataflow = serverless unified batch/stream ETL with Beam. Exam tip: match scenario keywords to the right service. |
