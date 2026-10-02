## Chapter 5: Data Fusion, Dataform, and Orchestration with Cloud Composer — Practice Questions

**Q1.** A retail company's business analysts (no coding experience) need to build a batch ETL pipeline pulling data from an on-prem Oracle database into BigQuery, using prebuilt connectors and a drag-and-drop UI. Which tool BEST fits this requirement?

A) Cloud Composer
B) Cloud Data Fusion
C) Dataform
D) A custom Dataflow Java pipeline

<details>
<summary>Answer</summary>

**Correct: B**

Data Fusion provides a visual, drag-and-drop pipeline studio with 150+ prebuilt connectors (including Oracle via JDBC plugins) built for non-coders, running on managed Dataproc under the hood.

**Why the others are wrong:**
- Composer orchestrates workflows via Python DAGs — requires coding and doesn't provide visual ETL design.
- Dataform only transforms data already in BigQuery using SQL/SQLX — it cannot extract from an on-prem Oracle source.
- A custom Dataflow pipeline requires Java/Python development, which contradicts the no-code requirement.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A pipeline in Cloud Composer must alert the team if a critical task does not finish within 2 hours of the DAG run's scheduled start time, even if the task eventually succeeds. Which Airflow feature should be configured on the task?

A) sla
B) execution_timeout
C) retry_delay
D) trigger_rule

<details>
<summary>Answer</summary>

**Correct: A**

The 'sla' parameter defines a timedelta measured from the DAG run's scheduled start; if the task isn't done by then, the scheduler fires an SLA miss and can invoke sla_miss_callback, independent of task success/failure.

**Why the others are wrong:**
- execution_timeout kills the task if its own runtime exceeds the value — it fails the task rather than just alerting on a missed deadline.
- retry_delay only controls the wait time between retry attempts after a failure.
- trigger_rule controls when a downstream task runs based on upstream task states, not timing SLAs.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A Pub/Sub subscription forwards undeliverable messages to a dead-letter topic after repeated processing failures. What is the minimum number of delivery attempts that can be configured before a message is forwarded to the dead-letter topic?

A) 5
B) 1
C) 10
D) 100

<details>
<summary>Answer</summary>

**Correct: A**

Pub/Sub requires a minimum of 5 delivery attempts (max 100) before a message is moved to the configured dead-letter topic, preventing premature dead-lettering of transient failures.

**Why the others are wrong:**
- 1 attempt is not allowed — Pub/Sub enforces a floor of 5 to avoid dead-lettering on the first transient error.
- 10 is a plausible-sounding number but not the enforced minimum.
- 100 is actually the maximum allowed value, not the minimum.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A team wants BigQuery-native SQL transformation pipelines with automatic dependency resolution, built-in data quality assertions, and version control via Git, without managing any compute infrastructure. Which tool should they use?

A) Cloud Data Fusion
B) Cloud Composer
C) Dataform
D) Dataproc Serverless for Spark

<details>
<summary>Answer</summary>

**Correct: C**

Dataform is purpose-built for BigQuery: SQLX files define transformations, ref() functions auto-generate the dependency DAG, assertions provide data quality tests, and it integrates natively with Git — with no cluster or instance to manage.

**Why the others are wrong:**
- Data Fusion is a general visual ETL tool that runs on Dataproc clusters and targets multi-source ingestion, not BigQuery-native SQL-only transforms.
- Composer orchestrates tasks across systems but doesn't provide SQL dependency graphs or built-in assertions itself.
- Dataproc Serverless for Spark runs Spark jobs, not declarative SQL transformation workflows.

</details>

---

**Q5.** A pipeline must: (1) trigger a Data Fusion batch pipeline to land raw data in BigQuery, (2) run a Dataform workflow to transform it, and (3) send a notification on failure, with retries and cross-tool task dependencies. Which service should orchestrate this?

A) Cloud Composer
B) Cloud Data Fusion pipeline triggers
C) Dataform workflow_settings.yaml
D) Cloud Scheduler with Cloud Functions

<details>
<summary>Answer</summary>

**Correct: A**

Composer (managed Airflow) is designed to orchestrate dependencies across heterogeneous GCP tools (Data Fusion, Dataform, BigQuery) with native operators, configurable retries, and failure-branch notification tasks.

**Why the others are wrong:**
- Data Fusion pipeline triggers only chain Data Fusion pipelines to each other, not external tools like Dataform.
- Dataform's workflow_settings.yaml only governs dependency order within Dataform itself, not external services.
- Cloud Scheduler + Functions can trigger jobs but lacks native retry/dependency/backfill semantics that Airflow DAGs provide for complex multi-tool orchestration.

</details>

---

**Q6.** In an Airflow DAG running on Cloud Composer, a task must retry automatically up to 3 times with exponentially increasing delay between attempts if it fails. Which default_args configuration achieves this?

A) {'retries': 3, 'retry_delay': timedelta(minutes=5)}
B) {'sla': timedelta(minutes=5), 'retries': 3}
C) {'trigger_rule': 'all_done', 'retries': 3}
D) {'retries': 3, 'retry_delay': timedelta(minutes=5), 'retry_exponential_backoff': True}

<details>
<summary>Answer</summary>

**Correct: D**

Setting retries=3 with retry_delay as a base and retry_exponential_backoff=True causes Airflow to exponentially increase the wait time between each of the 3 retry attempts.

**Why the others are wrong:**
- This retries 3 times but with a fixed delay each time — not exponential.
- sla only triggers alerting on missed deadlines; it does not control retry behavior.
- trigger_rule governs downstream task execution conditions, not retry backoff behavior.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** In a Dataform project, table B must run only after table A successfully completes, and the dependency should be resolved automatically without manually listing task order. How is this dependency declared?

A) Add A to B's 'depends_on' list in workflow_settings.yaml
B) Set trigger_rule: 'all_success' on table B
C) Call ${ref('A')} inside table B's SQLX query
D) Add a Composer sensor task waiting for table A

<details>
<summary>Answer</summary>

**Correct: C**

Referencing a source with ${ref('A')} inside a SQLX file both queries the table AND registers it as a dependency, so Dataform automatically builds and orders the dependency graph at compile time.

**Why the others are wrong:**
- Dataform does not use a manual 'depends_on' list in workflow_settings.yaml for table-level dependencies — ref() handles this declaratively.
- trigger_rule is an Airflow/Composer concept, not part of Dataform's SQLX dependency model.
- A Composer sensor is unnecessary overhead — Dataform resolves intra-workflow dependencies natively without external orchestration.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement BEST describes the underlying execution model that distinguishes Cloud Data Fusion pipelines from Dataform workflows?

A) Data Fusion pipelines execute as ephemeral Dataproc (or Data Fusion-managed) clusters running Spark under the hood, while Dataform compiles to SQL executed directly by BigQuery
B) Both Data Fusion and Dataform execute exclusively inside BigQuery's query engine
C) Data Fusion requires Airflow to execute pipelines, while Dataform runs on Cloud Functions
D) Dataform pipelines run on Dataproc Spark clusters, while Data Fusion executes SQL directly in BigQuery

<details>
<summary>Answer</summary>

**Correct: A**

Data Fusion pipelines are compiled and executed via Dataproc (Spark) clusters (ephemeral or persistent, depending on execution profile), while Dataform compiles its SQLX definitions into standard SQL executed natively by BigQuery — no separate compute cluster.

**Why the others are wrong:**
- Data Fusion does not execute exclusively in BigQuery — it typically provisions Dataproc clusters for pipeline runs.
- Data Fusion does not require Airflow to execute — Airflow/Composer only orchestrates triggering it externally.
- This reverses the actual architecture — Dataform runs SQL in BigQuery, not Spark on Dataproc.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

