## Chapter 2: Selecting Transformation Tools and Designing for Governance — Practice Questions

**Q1.** A retail company has 200 existing Spark jobs written in Scala that must be migrated to GCP within two weeks with minimal code rewrites. The team wants to keep using spark-submit and existing job scheduling scripts. Which service should they choose?

A) Dataflow, rewriting the jobs using the Apache Beam Spark runner
B) Data Fusion, rebuilding the pipelines visually with pre-built connectors
C) Dataform, converting the Spark logic into SQLX transformation files
D) Dataproc, lifting and shifting the existing Spark jobs onto managed clusters

<details>
<summary>Answer</summary>

**Correct: D**

Dataproc runs unmodified Hadoop/Spark jobs on managed clusters, making it the standard lift-and-shift target when timelines are short and code must stay as-is.

**Why the others are wrong:**
- Dataflow requires rewriting jobs in the Beam model — not a lift-and-shift, and too slow for a 2-week deadline.
- Data Fusion is a no-code visual tool; rebuilding 200 Scala jobs visually is far more work than moving clusters.
- Dataform only handles SQL-based BigQuery transformations, not general Spark compute jobs.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A business analytics team with no programming experience needs to build ETL pipelines that read from Cloud SQL, join with a CSV in Cloud Storage, and load into BigQuery, using a drag-and-drop interface with pre-built connectors.

A) Dataproc with PySpark notebooks
B) Dataform with SQLX scripts
C) Dataflow using the Apache Beam Python SDK
D) Data Fusion with its visual pipeline studio

<details>
<summary>Answer</summary>

**Correct: D**

Data Fusion (built on CDAP) provides a visual, no-code/low-code pipeline studio with pre-built plugins for sources like Cloud SQL and sinks like BigQuery — exactly suited to non-programmers.

**Why the others are wrong:**
- Dataproc notebooks require Spark/Python coding skills the team lacks.
- Dataform requires SQL scripting knowledge and Git workflows, not drag-and-drop.
- Dataflow requires writing Beam pipeline code in Java or Python.

</details>

---

**Q3.** An analytics engineering team wants to define BigQuery table transformations as version-controlled SQL with explicit dependency graphs, unit tests (assertions), and scheduled execution, without managing any compute clusters.

A) Dataform
B) Dataproc Serverless for Spark SQL
C) Data Fusion Enterprise edition
D) Dataflow SQL

<details>
<summary>Answer</summary>

**Correct: A**

Dataform is purpose-built for SQL-based ELT inside BigQuery: it compiles SQLX into dependency-ordered queries, supports Git version control, table assertions, and scheduled workflows — with BigQuery itself doing all compute.

**Why the others are wrong:**
- Dataproc Serverless for Spark runs Spark jobs, not native BigQuery SQL dependency graphs with assertions.
- Data Fusion is a visual ETL tool aimed at non-SQL users, not a SQL-first dependency/testing framework.
- Dataflow SQL runs streaming/batch Beam SQL pipelines, not BigQuery-native ELT with version control.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which GCP service should be selected for a pipeline that must process both unbounded streaming data and historical batch data using a single codebase, with fully managed autoscaling and no cluster provisioning?

A) Dataproc with autoscaling policies
B) Data Fusion
C) Cloud Composer
D) Dataflow

<details>
<summary>Answer</summary>

**Correct: D**

Dataflow is Google's fully managed Apache Beam runner, unifying batch and streaming in one programming model with serverless autoscaling — no clusters to manage.

**Why the others are wrong:**
- Dataproc requires managing Spark/Hadoop clusters, even with autoscaling policies enabled.
- Data Fusion generates pipelines that execute on Dataproc under the hood; it isn't itself the unified batch/stream runtime.
- Cloud Composer is an orchestration tool (managed Airflow), not a data processing engine.

</details>

---

**Q5.** A company stores data across BigQuery datasets and multiple Cloud Storage buckets and needs a single place to discover data assets, view automated data lineage between transformation jobs, and enforce consistent access policies across both storage systems.

A) Dataplex
B) Data Catalog alone
C) Dataform metadata graph
D) Cloud DLP

<details>
<summary>Answer</summary>

**Correct: A**

Dataplex provides unified governance across BigQuery and Cloud Storage: it organizes assets into lakes/zones, auto-generates lineage from jobs like Dataflow/Dataproc/BigQuery, and applies centralized IAM-based policies.

**Why the others are wrong:**
- Data Catalog (now part of Dataplex) provides discovery/metadata search but not lineage or zone-based policy enforcement on its own.
- Dataform's dependency graph only shows lineage within its own SQL transformations, not across GCS and other pipelines.
- Cloud DLP is for sensitive data discovery/redaction, not general lineage and cataloging.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** In an Apache Beam pipeline, you need to group streaming events into non-overlapping 10-minute buckets based on event time. Which windowing strategy should you apply?

A) Session windows with a 10-minute gap duration
B) Fixed windows of 10 minutes
C) Sliding windows of size 10 minutes with a 5-minute period
D) Global window with a repeating trigger every 10 minutes

<details>
<summary>Answer</summary>

**Correct: B**

Fixed (tumbling) windows partition a PCollection into equal, non-overlapping time intervals — exactly a 10-minute non-overlapping bucket requirement.

**Why the others are wrong:**
- Session windows group events by activity gaps, not fixed calendar buckets, and would overlap conceptually with idle-time logic instead.
- Sliding windows overlap by design (a 5-minute period on a 10-minute window means windows overlap), violating the non-overlapping requirement.
- Global window with a periodic trigger still uses one window for all data; it doesn't create discrete time-bounded buckets by event time.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** You are configuring a Dataproc cluster for a workload with highly variable job load. To minimize cost while ensuring jobs don't queue for long periods, what should you configure?

A) A single fixed-size cluster sized for peak load, running continuously
B) A Dataproc cluster with preemptible-only primary workers and no secondary workers
C) A Dataproc autoscaling policy with defined min/max worker instances attached to an ephemeral cluster
D) Manual resizing via gcloud commands triggered by Cloud Monitoring alerts every hour

<details>
<summary>Answer</summary>

**Correct: C**

Dataproc autoscaling policies let you set min/max secondary worker counts and scale automatically based on YARN pending memory, matching variable load cost-efficiently on ephemeral, job-scoped clusters — the documented best practice.

**Why the others are wrong:**
- A fixed peak-sized cluster wastes cost during low-load periods.
- Preemptible-only primary workers is not supported/recommended — primary workers must be standard, and this doesn't address autoscaling for variable load.
- Manual hourly resizing is slow, operationally heavy, and not responsive to real-time load spikes compared to native autoscaling.

</details>

---

**Q8.** Which statement best describes the data mesh principle as implemented using Dataplex on Google Cloud?

A) A single centralized team owns all data pipelines and enforces schema for every domain
B) All data must be copied into one physical BigQuery project to simplify access control
C) Data mesh eliminates the need for any data governance tooling since domains self-manage independently
D) Domain teams own their data as products within Dataplex lakes/zones, while central governance policies are applied federally across all domains

<details>
<summary>Answer</summary>

**Correct: D**

Data mesh decentralizes data ownership to domain teams (modeled as Dataplex lakes/zones) who treat data as a product, while a federated governance layer (Dataplex policies, IAM, data quality) still applies consistent standards across domains.

**Why the others are wrong:**
- Centralized single-team ownership is the opposite of data mesh's domain-oriented decentralization.
- Data mesh does not require physically copying data into one project — it favors decentralized storage with federated access, which Dataplex supports without data movement.
- Data mesh still requires federated governance; it removes centralized bottlenecks, not governance itself.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

