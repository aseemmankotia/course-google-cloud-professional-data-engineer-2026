## Chapter 1: Choosing the Right Storage and Processing Architecture — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | BigQuery: max number of partitions per partitioned table? | 10,000 partitions per table. Exam tip: exceeding this means you need a coarser partition granularity (e.g., month instead of day) or clustering instead. |
| 2 | BigQuery: max number of clustering columns per table? | 4 columns, order matters — put the most frequently filtered/highest-selectivity column first. |
| 3 | Cloud SQL: max storage per instance? | 64TB (MySQL/PostgreSQL). Exam tip: beyond this scale, or for cross-region strong consistency, choose Spanner instead. |
| 4 | Pub/Sub message retention: default and max? | Default 7 days; configurable up to 31 days. Exam tip: used for replay/backfill in streaming pipelines. |
| 5 | Cloud Spanner SLA: regional vs multi-region? | 99.99% regional, 99.999% multi-region. Exam tip: multi-region is the only GCP DB SLA reaching 5 nines. |
| 6 | Bigtable vs Spanner — when to choose which? | Bigtable: NoSQL wide-column, single-row key lookups, huge write throughput, no multi-row ACID. Spanner: relational, multi-row/table ACID transactions, SQL, horizontal scale. Exam tip: 'joins/ACID across rows' → Spanner; 'time-series/IoT/single-digit ms key lookup' → Bigtable. |
| 7 | BigQuery vs Cloud SQL — decision rule? | BigQuery: OLAP, ad-hoc SQL over TB–PB scale, serverless. Cloud SQL: OLTP, transactional app backend, <64TB. Exam tip: scenario mentioning 'analytics/reporting/data warehouse' → BigQuery; 'application backend/transactions' → Cloud SQL. |
| 8 | Firestore vs Bigtable — decision rule? | Firestore: mobile/web client SDKs, offline sync, real-time listeners, document model, moderate scale. Bigtable: massive throughput, no client SDK/offline sync, wide-column analytical/operational workloads. Exam tip: 'mobile app' or 'offline sync' → Firestore. |
| 9 | BigQuery SQL: syntax to partition a table by date column? | `CREATE TABLE dataset.table (col1 STRING, event_date DATE) PARTITION BY event_date CLUSTER BY user_id;` Exam tip: PARTITION BY accepts DATE, TIMESTAMP (by day/hour/month/year), or an INTEGER range. |
| 10 | Which BigQuery write API is recommended over legacy streaming insert (tabledata.insertAll)? | The Storage Write API — lower cost, exactly-once semantics, higher throughput. Exam tip: legacy streaming insert has per-row size limits and no free tier; Storage Write API is the current best practice. |
| 11 | Dataflow windowing: fixed vs sliding vs session windows? | Fixed: non-overlapping, equal time intervals. Sliding: overlapping windows with a period < window size. Session: gap-based, groups events with inactivity gaps. Exam tip: 'user session analysis' → session windows. |
| 12 | Lambda vs Kappa architecture? | Lambda: separate batch layer (accuracy) + speed layer (low latency), merged at serving layer — more complex, two codebases. Kappa: single streaming pipeline reprocesses history by replaying the log — simpler, one codebase. Exam tip: GCP favors Kappa via Dataflow + Pub/Sub replay for new streaming-first designs. |
