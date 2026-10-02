## Chapter 5: Data Fusion, Dataform, and Orchestration with Cloud Composer — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Pub/Sub dead-letter topic: minimum and maximum delivery attempts allowed before forwarding? | Min 5, Max 100. Exam tip: below 5 is invalid config — prevents dead-lettering on the very first transient failure. |
| 2 | Default value of 'retries' in Airflow default_args if not specified? | 0 — tasks do NOT retry automatically unless you explicitly set retries. Exam tip: exam scenarios about 'task keeps failing without retry' usually mean retries wasn't set. |
| 3 | Data Fusion vs Dataform — core use case difference? | Data Fusion = visual, multi-source ETL/ELT (on-prem, DB, files → BigQuery), runs on Dataproc. Dataform = SQL-only transformation of data already in BigQuery, no cluster needed. |
| 4 | Cloud Data Fusion editions and their key differentiator? | Basic (limited, no HA, no versioning), Enterprise (full features, HA, real-time pipelines), Developer (single-node, dev/test only, not for production). |
| 5 | Dataform: how do you declare a dependency between two SQLX transformations? | Use ${ref('table_name')} inside the SQLX query — this both queries the table and auto-registers it in the dependency DAG. |
| 6 | Airflow default_args syntax for exponential retry backoff? | {'retries': 3, 'retry_delay': timedelta(minutes=5), 'retry_exponential_backoff': True} — delay grows exponentially with each retry attempt. |
| 7 | Name common Airflow trigger_rule values used for error-handling branches. | all_success (default), all_failed, one_failed, all_done, none_failed. Use 'one_failed' or 'all_done' to build a dead-letter/failure-notification branch task. |
| 8 | Architecture pattern: orchestrating Data Fusion + Dataform + BigQuery load in one pipeline — which service ties them together? | Cloud Composer (Airflow) — uses operators/sensors to trigger Data Fusion pipelines, then Dataform workflow invocations, with retries and failure branches across tools. |
| 9 | Dead-letter pattern in a Dataflow streaming pipeline — how is it typically implemented? | Use TupleTags to split output into a 'main' PCollection and a 'dead-letter' PCollection for records that fail parsing/validation, then write the dead-letter output to a separate BigQuery table or Pub/Sub topic for review. |
| 10 | Cloud Composer 2 environment size options? | Small, Medium, Large — controls scheduler/worker/web server resource allocation and autoscaling ranges. Exam tip: worker autoscaling min/max is configured independently of environment size. |
| 11 | Dataform pricing model? | Dataform itself has no separate compute charge — you only pay for the BigQuery query/slot costs it triggers when running compiled SQL. |
| 12 | Airflow 'sla' parameter — what does it measure against? | Time elapsed since the DAG run's scheduled start (execution_date), not the task's own start time. Missing it triggers sla_miss_callback/alert — it does NOT fail or kill the task. |
