## Chapter 10: Operating and Monitoring Production Data Workloads — Practice Questions

**Q1.** A streaming Dataflow pipeline ingests from Pub/Sub, groups events by user_id, and aggregates counts with Combine.perKey(). Monitoring shows one worker pinned at 100% CPU while others sit near 10%, and system lag keeps climbing. What should you do FIRST to fix this?

A) Increase the worker machine type to give the hot worker more CPU
B) Add a random salt to user_id and use a second Combine.perKey() stage (or CombineFn with fanout) to spread the hot key across workers
C) Increase maxNumWorkers so Dataflow autoscales to more workers
D) Switch the pipeline from Dataflow to Dataproc Structured Streaming

<details>
<summary>Answer</summary>

**Correct: B**

A single busy worker with low overall lag on the rest is the classic hot-key signature. Autoscaling and bigger machines don't help because the problem is all the traffic for one key landing on one worker/shard — you must redistribute the key with salting + a two-stage combine (or Combine.withFanout).

**Why the others are wrong:**
- Bigger machine type only helps if the bottleneck is CPU-bound work spread across keys, not a single hot key
- More workers won't help — Dataflow can't split a single key's GroupByKey/Combine work across additional workers without code changes
- Migrating to Dataproc doesn't fix hot-key skew; the same partitioning problem would reappear in Spark

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A company's BigQuery dataset is created in the 'US' multi-region. New compliance rules require the ability to fail over analytics workloads to the 'EU' multi-region with an acceptable RPO of a few hours. Which approach BEST meets this requirement?

A) Enable a dataset-level replication toggle to make the US dataset automatically mirror to EU
B) Rely on BigQuery time travel to recover data in the EU region if the US region fails
C) Schedule regular BigQuery Data Transfer Service (or scheduled query/export+load) jobs that copy tables from the US dataset into an EU dataset
D) Recreate the same dataset name in the EU multi-region and expect BigQuery to sync automatically

<details>
<summary>Answer</summary>

**Correct: C**

BigQuery dataset location is immutable and there is no built-in cross-region replication for datasets, so scheduled copy/export-load jobs (via BigQuery Data Transfer Service, scheduled queries, or Dataflow) into an EU dataset is the standard DR pattern for cross-region failover.

**Why the others are wrong:**
- No such automatic dataset replication toggle exists between multi-regions in BigQuery
- Time travel only protects against accidental deletes/updates within the same region and dataset for up to 7 days — it does not provide cross-region DR
- Creating a dataset with the same name in another region does not create any data sync; you must explicitly copy the data

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A Cloud Composer DAG must alert the on-call team whenever the daily ETL pipeline's end-to-end SLA of 2 hours is missed. What is the correct way to configure this?

A) Set an `sla` timedelta on the relevant tasks/DAG and provide an `sla_miss_callback` to send the alert
B) Create a Cloud Monitoring uptime check against the Composer web server URL
C) Add a BigQuery scheduled query that runs every 2 hours and checks a status table
D) Manually review Cloud Logging entries at the end of each business day

<details>
<summary>Answer</summary>

**Correct: A**

Airflow (and therefore Composer) has native SLA support: define `sla` on tasks/DAGs and Airflow evaluates it against the schedule, firing `sla_miss_callback` and logging SLA misses automatically — this is the purpose-built mechanism for pipeline SLA alerting.

**Why the others are wrong:**
- Uptime checks test HTTP endpoint availability, not whether a DAG's business logic completed within a time budget
- A scheduled query checking a status table is a workaround that duplicates functionality Airflow already provides and adds latency/complexity
- Manual log review is not automated alerting and fails the 'monitoring/alerting configuration' exam objective

</details>

---

**Q4.** You need cross-region disaster recovery for a Cloud Storage bucket with an RPO target of about 15 minutes, without building any custom replication logic. Which configuration should you choose?

A) A standard multi-region bucket
B) A regional bucket with a nightly `gsutil rsync` cron job
C) Storage Transfer Service running a daily batch job between two regional buckets
D) A dual-region bucket with Turbo Replication enabled

<details>
<summary>Answer</summary>

**Correct: D**

Turbo Replication is only available on dual-region buckets and provides an SLA that replicates 100% of new objects to the second region within 15 minutes, which is the documented mechanism for a near-real-time cross-region RPO target.

**Why the others are wrong:**
- Standard multi-region buckets replicate within the geographic multi-region but don't offer a documented 15-minute RPO SLA and aren't the same as dual-region + Turbo Replication
- Nightly rsync gives an RPO of up to 24 hours, far worse than 15 minutes
- A daily Storage Transfer Service job also gives an RPO of up to 24 hours, not 15 minutes

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** Your team wants to detect gradual degradation in pipeline freshness (e.g., data arriving later and later each day) before it breaches the SLA, and alert only when the trend threatens to exhaust the allowed error budget. Which Cloud Monitoring feature should you use?

A) A single static threshold alerting policy on a log-based metric
B) Cloud Trace latency sampling
C) Error Reporting group notifications
D) A Service Level Objective (SLO) with a burn-rate alerting policy

<details>
<summary>Answer</summary>

**Correct: D**

SLOs with burn-rate alerts are designed exactly for this: they track how fast the error budget is being consumed and can fire early warnings before a hard SLA breach, rather than only alerting on a fixed instantaneous threshold.

**Why the others are wrong:**
- A static threshold only fires after the value crosses the line; it doesn't account for budget consumption rate or forecast future breaches
- Cloud Trace is for distributed request latency tracing, not pipeline freshness SLAs
- Error Reporting aggregates application exceptions/stack traces, not freshness/latency SLO burn rates

</details>

---

**Q6.** You need to configure BigQuery's time travel window to the maximum value it supports so you can recover accidentally deleted rows from up to that many days ago. What value should you configure?

A) 7 days
B) 2 days
C) 14 days
D) 30 days

<details>
<summary>Answer</summary>

**Correct: A**

BigQuery time travel is configurable per dataset between 2 and 7 days, with 7 days being the maximum window and the default.

**Why the others are wrong:**
- 2 days is the minimum configurable value, not the maximum
- 14 days exceeds BigQuery's supported time travel range
- 30 days exceeds BigQuery's supported time travel range; long-term retention beyond 7 days requires table snapshots instead

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A Spark job on Dataproc is stuck: 199 of 200 shuffle partitions finish in seconds, but one partition takes over an hour and job progress stalls. Profiling shows one join key has far more rows than any other. Which fix directly addresses this?

A) Enable Dataproc autoscaling to add more secondary workers
B) Switch all workers to preemptible VMs to reduce cost while waiting
C) Increase the cluster's disk size to give the shuffle more local storage
D) Salt the skewed join key and increase shuffle partitions (e.g., raise `spark.sql.shuffle.partitions`) so the large key's rows are spread across more tasks

<details>
<summary>Answer</summary>

**Correct: D**

This is classic data skew: one key dominates a partition. Salting the key plus increasing shuffle partitions redistributes the work across more tasks/executors, which directly fixes the imbalance.

**Why the others are wrong:**
- Autoscaling adds capacity but can't split a single oversized partition's work across more tasks without changing partitioning logic
- Preemptible workers reduce cost but do nothing for skew, and risk losing the already-slow task's progress
- More disk space addresses spill-to-disk I/O issues, not the root cause of key skew

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the key difference between using `gcloud dataflow jobs drain` versus `gcloud dataflow jobs cancel` on a running streaming Dataflow pipeline?

A) Drain stops ingesting new data and finishes processing already-buffered data before shutting down, while cancel stops the pipeline immediately and may leave in-flight data unprocessed
B) Drain immediately terminates all workers, while cancel waits for in-flight data to be processed
C) Drain and cancel behave identically for streaming pipelines but differ only for batch pipelines
D) Drain deletes the pipeline's Cloud Storage staging files, while cancel preserves them

<details>
<summary>Answer</summary>

**Correct: A**

Draining a job closes off new input, lets buffered/in-flight elements finish processing and flush watermarks/state, then shuts down gracefully — the recommended way to stop a pipeline without data loss. Cancel halts execution immediately, which can drop in-flight and buffered data.

**Why the others are wrong:**
- This reverses the actual behavior — drain is the graceful option, cancel is the immediate one
- The behaviors differ specifically for streaming jobs; for batch jobs drain and cancel behave the same (drain immediately cancels batch jobs)
- Neither command's primary distinction is about staging file cleanup

</details>

---

