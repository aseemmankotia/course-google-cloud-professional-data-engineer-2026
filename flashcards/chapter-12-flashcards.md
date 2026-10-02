## Chapter 12: Full Exam Simulation and Test-Taking Strategy — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | GCP Professional Data Engineer exam: total questions and time limit? | ~50-60 questions, 120 minutes — budget ~2 min/question. Exam tip: flag ambiguous questions and move on rather than burning time. |
| 2 | BigQuery max partitions per time-unit partitioned table? | 4,000 partitions. Exam tip: 'daily partitions over many years' scenarios can exceed this — watch for the trap. |
| 3 | Dataflow vs Dataproc — when to choose which? | Dataflow: serverless, unified batch/stream (Apache Beam), autoscaling, minimal ops. Dataproc: managed Hadoop/Spark clusters, choose when migrating existing Spark/Hadoop jobs or needing cluster-level control. |
| 4 | Bigtable vs Firestore — selection criteria? | Bigtable: high-throughput wide-column, time-series/IoT, single-key lookups, no multi-row ACID. Firestore: document store, strong consistency, multi-document ACID transactions, mobile/web offline sync. |
| 5 | bq CLI command to load CSV with schema autodetect? | bq load --autodetect --source_format=CSV dataset.table gs://bucket/file.csv — Exam tip: autodetect can misinfer types; know when to specify an explicit schema instead. |
| 6 | Pub/Sub message retention: default and maximum? | Default 7 days (604800s), configurable up to 31 days max. Exam tip: retention is set per-subscription, not per-topic. |
| 7 | Elimination technique for two close-looking answer choices? | Eliminate options with absolute wording (always/never), deprecated services, or that violate a stated constraint (cost, latency, compliance, ops overhead). Compare the remaining options against the exact requirement wording. |
| 8 | Cloud Storage class: data accessed <1x/year vs ~1x/month? | <1x/year → Archive (~$0.0012/GB/mo, 365-day min); ~1x/month → Nearline (~$0.01/GB/mo, 30-day min). Exam tests minimum storage duration and early-deletion fees. |
| 9 | Time-management rule for long scenario questions? | Read the LAST sentence (the actual ask) first, then skim requirements. Avoids anchoring on irrelevant details and saves re-reading time. |
| 10 | IAM trap: bigquery.jobUser vs bigquery.dataViewer? | jobUser lets a user run jobs/queries but does NOT grant read access to data; dataViewer grants read-only data access. Common wrong answer: granting dataEditor when only jobUser+dataViewer is needed. |
| 11 | Dataflow windowing types tested on the exam? | Fixed (regular non-overlapping intervals), Sliding (overlapping intervals), Session (gap-duration based). Session windows are the answer for grouping bursty activity with variable gaps. |
| 12 | Flag-and-revisit exam strategy — correct approach? | Always answer with your best guess immediately (never leave blank), flag for review, keep pacing. Only return to flagged questions after all questions are attempted, using remaining time. |
