## Chapter 4: Batch ETL, Dataproc, and Change Data Capture — Practice Questions

**Q1.** A company runs nightly Dataproc Spark jobs processing 2TB of data. The cluster currently runs 24/7 even though jobs only execute for 90 minutes each night. Which change BEST reduces cost while keeping the same SLA?

A) Increase the persistent disk size on all worker nodes
B) Convert the cluster to a single-node cluster
C) Use a Workflow Template to create an ephemeral cluster before the job starts and delete it after the job completes
D) Switch all primary workers to preemptible VMs but keep the cluster running continuously

<details>
<summary>Answer</summary>

**Correct: C**

Ephemeral clusters created via Workflow Templates exist only for the job's duration, eliminating idle-cluster cost while preserving the same compute capacity and SLA for the batch window.

**Why the others are wrong:**
- Disk size does not address the 24/7 idle-cluster cost problem
- Single-node clusters remove HA/parallelism and don't solve the idle-time cost issue
- Primary workers cannot be preemptible/Spot, and keeping the cluster always-on still wastes cost during idle hours

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A retail company needs to replicate change events from an on-premises Oracle database to BigQuery with minimal latency and without adding polling load to the source. Which service should they use?

A) A nightly Cloud Data Fusion batch pipeline
B) A Dataproc Spark job polling via JDBC every 15 minutes
C) Datastream using log-based CDC
D) Storage Transfer Service

<details>
<summary>Answer</summary>

**Correct: C**

Datastream reads Oracle redo logs (log-based CDC) to stream changes continuously with low latency and negligible source impact.

**Why the others are wrong:**
- Nightly batch pipelines introduce hours of latency and don't capture row-level changes
- JDBC polling adds repeated query load to the source and misses changes between polls
- Storage Transfer Service moves files/objects, not database change events

</details>

---

**Q3.** A batch pipeline occasionally fails partway through and is automatically retried. The downstream BigQuery table must never contain duplicate rows after a retry. Which design BEST guarantees this?

A) Use BigQuery streaming inserts with randomly generated row IDs
B) Truncate the target table and fully reload it on every single retry regardless of failure point
C) Append rows on every retry and deduplicate later using SELECT DISTINCT in downstream queries
D) Load data into a staging table, then run a MERGE statement keyed on a business/primary key into the target table

<details>
<summary>Answer</summary>

**Correct: D**

Staging + MERGE keyed on a stable business key makes reruns produce an identical end state (upsert semantics) regardless of how many times the job restarts — the definition of idempotency.

**Why the others are wrong:**
- Random row IDs on retry do not prevent duplicate business-key rows
- Full reload on every retry is wasteful and doesn't scale, and isn't always safe if source data changed mid-failure
- DISTINCT masks duplicates in queries but leaves the underlying table corrupted and doesn't fix write-side idempotency

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A team needs CDC from a self-managed MySQL 8 database on Compute Engine into BigQuery, including handling of new columns added over time. Which GCP service is purpose-built for this?

A) Dataflow with a custom JDBC connector
B) Datastream
C) Cloud Data Fusion
D) Pub/Sub

<details>
<summary>Answer</summary>

**Correct: B**

Datastream natively supports MySQL as a CDC source using binlog-based log capture and includes automatic schema drift detection for new/dropped columns.

**Why the others are wrong:**
- Dataflow with custom JDBC requires building CDC and schema-drift logic manually
- Data Fusion is primarily a batch/graphical ETL tool, not a native log-based CDC service
- Pub/Sub is a messaging bus, not a database CDC extraction service

</details>

---

**Q5.** Multiple analysts need a shared interactive Spark/Jupyter environment available throughout the business day. Which Dataproc cluster approach fits best?

A) An ephemeral cluster created per job through Cloud Composer
B) A Dataproc Serverless batch submitted per query
C) A persistent standard cluster with Component Gateway enabled for Jupyter access
D) A cluster deleted immediately after each notebook cell run

<details>
<summary>Answer</summary>

**Correct: C**

Persistent/long-running clusters with Component Gateway are the correct pattern for shared, ongoing interactive workloads like notebooks.

**Why the others are wrong:**
- Ephemeral per-job clusters are unsuitable for continuous interactive use — they add startup latency each time
- Dataproc Serverless batches are for discrete jobs, not interactive shared sessions
- Deleting a cluster after every cell run destroys the interactive session state and is impractical

</details>

---

**Q6.** You want to reduce cost for a fault-tolerant Spark batch job on Dataproc using discounted VMs, but you must avoid job failure risk since master/core capacity could be reclaimed. What is the BEST configuration?

A) Configure all workers, including primary workers, as preemptible/Spot VMs
B) Use only primary (standard) workers and no secondary workers at all
C) Use only persistent standard workers combined with a committed use discount
D) Use standard primary workers plus preemptible/Spot secondary workers, with graceful decommissioning enabled

<details>
<summary>Answer</summary>

**Correct: D**

Standard primary workers guarantee cluster stability (HDFS/YARN core services), while preemptible/Spot secondary workers cut cost for extra compute capacity; graceful decommissioning lets tasks finish before a secondary worker is reclaimed.

**Why the others are wrong:**
- Primary workers cannot safely be preemptible — losing them risks cluster/job failure
- No secondary workers removes the cost-saving opportunity entirely
- Committed use discounts save cost but don't provide the same steep discount as preemptible/Spot, and don't address the scenario's cost-reduction goal as effectively

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** In a Dataproc autoscaling policy, which setting controls the minimum time the cluster waits after a scaling action before it can scale again?

A) Graceful decommission timeout
B) Idle deletion timeout
C) Cluster TTL (time-to-live)
D) Cooldown period

<details>
<summary>Answer</summary>

**Correct: D**

The cooldown period (default 2 minutes) prevents rapid, thrashing scale-up/scale-down cycles by enforcing a minimum wait between scaling events.

**Why the others are wrong:**
- Graceful decommission timeout controls how long a worker being removed waits for running tasks to finish, not scaling frequency
- Idle deletion timeout deletes the entire cluster after it sits idle, unrelated to autoscaling cadence
- Cluster TTL deletes the cluster after a fixed lifetime, not related to scaling cadence

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the key architectural difference between an ephemeral Dataproc cluster and a persistent Dataproc cluster?

A) Ephemeral clusters cannot use preemptible or Spot VMs
B) Persistent clusters are always cheaper overall than ephemeral clusters
C) Ephemeral clusters are created for a single job or workflow and torn down afterward, avoiding idle cost, while persistent clusters run continuously for shared/interactive workloads
D) Ephemeral clusters cannot read from or write to Cloud Storage

<details>
<summary>Answer</summary>

**Correct: C**

This is the core distinction tested on the exam: ephemeral = per-job lifecycle, no idle billing; persistent = always-on for shared/interactive use, with idle cost unless managed via autoscaling/idle deletion.

**Why the others are wrong:**
- Ephemeral clusters can absolutely use preemptible/Spot secondary workers
- Persistent clusters typically cost more overall due to idle time, not less
- Both cluster types read/write Cloud Storage via the GCS connector identically

</details>

---

