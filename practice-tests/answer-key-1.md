# Answer Key — Practice Test 1

**Q1. Correct: C**

Bigtable is a wide-column, key-value NoSQL database purpose-built for very high-throughput writes and single-digit millisecond latency at petabyte scale, which matches the sensor telemetry workload exactly.

---

**Q2. Correct: A**

Dataflow with Pub/Sub provides a fully managed, autoscaling streaming architecture with built-in exactly-once semantics and native windowing (fixed, sliding, session), enabling sub-second-to-low-second latency decisioning.

---

**Q3. Correct: B**

Partitioning by order_date lets BigQuery prune partitions outside the 30-day range, and clustering by customer_id further sorts data within each partition so equality filters on customer_id skip irrelevant blocks, minimizing scanned bytes and cost.

---

**Q4. Correct: A**

Dataproc is managed Hadoop/Spark on GCP, allowing existing Spark and MLlib jobs to run with little to no code changes, supporting HDFS-compatible storage via Cloud Storage connectors.

---

**Q5. Correct: D**

Cloud Spanner provides horizontally scalable, strongly consistent relational SQL with multi-region configurations offering up to 99.999% availability, uniquely satisfying all stated requirements.

---

**Q6. Correct: D**

Dataplex is Google Cloud's unified data governance service, providing automated discovery, cataloging, lineage, and centrally managed fine-grained security policies across BigQuery, Cloud Storage, and other sources, with native support for organizing data into logical domains for data mesh architectures.

---

**Q7. Correct: D**

Prefixing with device_id distributes write load across many row-key prefixes (one per device), avoiding hotspots on a single tablet server, while appending a reverse timestamp keeps each device's most recent readings sorted first, enabling fast, efficient range scans for 'latest readings per device'.

---

**Q8. Correct: A**

Dataproc provides managed Hadoop, Spark, Hive, and Pig clusters with near-zero code changes required for lift-and-shift migrations, supports ephemeral, autoscaling, job-scoped clusters, and gives fine-grained control over cluster configuration and versions.

---

**Q9. Correct: A**

Date/timestamp partitioning on event_date directly matches the date-range filter used in every query, enabling BigQuery to prune entire partitions before scanning. Clustering by customer_id then sorts data within each partition, letting BigQuery skip blocks that don't match the customer_id filter — this combination minimizes bytes scanned.

---

**Q10. Correct: B**

Cloud Spanner's multi-region configurations provide externally consistent, ACID multi-row transactions with a SQL interface and horizontal scalability across continents, which is exactly what this global ledger requires.

---

**Q11. Correct: B**

Setting withAllowedLateness(10 minutes) combined with accumulating-mode triggers lets the pipeline emit an early result at the watermark and then update/re-fire the pane when late data arrives within the allowed lateness window, correctly incorporating late events while still bounding how long the pipeline waits.

---

**Q12. Correct: C**

Datastream is Google Cloud's serverless CDC and replication service, purpose-built to capture change streams from sources like Oracle and MySQL and stream them directly into BigQuery (or GCS) in near real time without managing infrastructure.

---

**Q13. Correct: C**

Dataproc Serverless for Spark runs batch Spark workloads without provisioning or managing any cluster; Google Cloud automatically provisions and tears down resources per job, so you pay only for the ~45 minutes of actual compute, matching the cost-minimization and zero-management requirements exactly.

---

**Q14. Correct: D**

Catching parsing exceptions inside the DoFn and emitting bad records to a side output routed to a dead-letter topic (or BigQuery table) allows the pipeline to continue processing valid records instead of endlessly retrying on poison messages, while preserving the malformed records for later inspection and reprocessing — the standard Beam/Dataflow error-handling pattern.

---

**Q15. Correct: B**

Dataform is Google Cloud's managed service purpose-built for defining SQL-based ELT transformation workflows in BigQuery, providing built-in version control (Git integration), dependency graphs (DAGs) between SQL tables, and assertions for data quality testing — with no cluster or infrastructure to manage.

---

**Q16. Correct: C**

Pub/Sub itself only guarantees at-least-once delivery, but Dataflow's streaming runner performs record-level deduplication (tracking record IDs, including Pub/Sub message IDs) so that, combined with its exactly-once state and output commit protocol, the pipeline delivers exactly-once processing semantics for the data flowing through transforms and sinks.

---

**Q17. Correct: A**

Datastream is a serverless, log-based CDC service that reads the MySQL binlog directly, capturing inserts/updates/deletes with minimal impact on the source database, and streams them to BigQuery (via Dataflow templates or the Datastream-to-BigQuery integration) with low latency.

---

**Q18. Correct: C**

Setting allowed lateness to 10 minutes keeps the 5-minute window's state open long enough to incorporate late data, while an accumulating trigger emits updated panes as late data arrives; once allowed lateness expires, Dataflow garbage-collects the window state, bounding memory growth.

---

**Q19. Correct: A**

Ephemeral, autoscaling Dataproc clusters orchestrated by Cloud Composer allow existing Spark/Hadoop code to run largely unchanged (using the GCS connector instead of HDFS), while clusters are created per job and deleted afterward, minimizing idle compute cost — the classic 'lift-and-shift' pattern for the exam.

---

**Q20. Correct: B**

GCSObjectExistenceSensor is the purpose-built Airflow sensor for waiting on a GCS object; running it in deferrable mode releases the worker slot while waiting (using the triggerer component), and it natively supports timeout and poke_interval/retry configuration — exactly matching the requirements.

---

**Q21. Correct: A**

The standard Beam dead-letter pattern catches parsing exceptions inside the DoFn and emits failures to a separate output (via TupleTag/additional outputs or a dead-letter PCollection), which can be written to a dead-letter Pub/Sub topic or BigQuery table for inspection, while the main pipeline continues processing valid records without stalling.

---

**Q22. Correct: A**

Placing the high-cardinality device_id first spreads writes evenly across the row-key space and thus across tablets/nodes, since monotonically increasing timestamps as the leading key element concentrate writes on the same tablet range at any given moment.

---

**Q23. Correct: C**

Date-based partitioning prunes irrelevant date ranges for the dominant query pattern, and clustering on customer_id further prunes blocks within the matching partitions, which also helps the occasional customer-only queries by skipping non-matching blocks.

---

**Q24. Correct: D**

This layered lifecycle matches each access-frequency phase to the cheapest class that still meets minimum storage duration requirements: Standard for frequent early access, Nearline for occasional access (30-day minimum), and Archive for the long-term rarely-accessed compliance retention (365-day minimum), with automatic deletion at the 7-year mark.

---

**Q25. Correct: C**

Cloud Spanner natively provides horizontally scalable, strongly consistent relational storage with automatic sharding, multi-region configurations offering up to 99.999% availability, and scale well beyond single-instance database limits.

---

**Q26. Correct: D**

BigQuery's default time travel window is 7 days; after that, a separate fail-safe period (an additional 7 days) retains deleted table data, but recovery from fail-safe is not self-service via SQL and requires opening a support case with Google Cloud.

---

**Q27. Correct: A**

For sustained, predictable, high-volume query workloads, capacity-based pricing (slot reservations via BigQuery Editions) provides flat, predictable costs that are significantly cheaper than paying per-TB-scanned at this scale.

---

**Q28. Correct: B**

Bigtable distributes rows across tablets by lexicographic key order. A monotonically increasing timestamp prefix forces all concurrent writes into the same narrow key range and tablet, causing hotspotting. Promoting sensorID (or a hash of it) to the key prefix spreads writes across many different key ranges/tablets while preserving time-ordered scans per sensor.

---

**Q29. Correct: C**

Cloud Storage lifecycle rules support combining the "age" condition (days since object creation) with "matchesStorageClass" to restrict the rule to objects currently in a specific storage class, paired with a SetStorageClass action targeting NEARLINE — exactly matching the stated requirement.

---

**Q30. Correct: A**

Cloud Spanner is Google Cloud's horizontally scalable, globally distributed relational database that provides strong consistency, multi-row ACID transactions, and full SQL support, with multi-region configurations designed exactly for this kind of scalable transactional workload.

---

**Q31. Correct: D**

BigQuery ML lets the team train and evaluate a linear regression model using only SQL, directly on data already in BigQuery, with no data movement and no need for Python/ML framework expertise.

---

**Q32. Correct: C**

Vertex AI Feature Store is purpose-built for this scenario: it provides centralized feature management, low-latency online serving, batch serving for training, feature reuse across teams, and built-in drift monitoring.

---

**Q33. Correct: D**

Analytics Hub lets data publishers create exchanges and listings on top of BigQuery datasets, allowing subscribers to query shared data directly without copying it, while the publisher retains centralized access control.

---

**Q34. Correct: B**

The TRANSFORM clause in CREATE MODEL specifies preprocessing steps that BigQuery ML automatically re-applies during ML.PREDICT, guaranteeing training and serving use identical transformations and eliminating train-serve skew.

---

**Q35. Correct: A**

Switching to a live/direct query connection means Looker Studio queries BigQuery directly on each dashboard load or refresh, reflecting the latest data instead of a periodically cached extract snapshot.

---

**Q36. Correct: C**

BigQuery table snapshots create an immutable, point-in-time copy of a table that persists independently of the source table's changes and beyond the default time travel window, giving a reliable, queryable reference for reproducible training data.

---

**Q37. Correct: D**

Vertex AI Feature Store is purpose-built to ingest features from BigQuery and provide both batch (offline) access for training and low-latency online serving, ensuring training-serving consistency, which directly addresses the requirement.

---

**Q38. Correct: A**

Wiring task failure callbacks into Cloud Monitoring/Pub/Sub and attaching an alerting policy provides near-real-time, proactive notification the moment a task fails, satisfying the SLA-visibility requirement.

---

**Q39. Correct: D**

Cloud DLP (Sensitive Data Protection) provides built-in infoType detectors for SSNs and credit card numbers and can redact or tokenize matched values in unstructured text before it is loaded, directly meeting the requirement.

---

**Q40. Correct: B**

Column-level security via policy tags lets you tag the sensitive column once and control access centrally through IAM on the taxonomy; access enforcement happens automatically on every query without maintaining duplicate views or exports.

---

**Q41. Correct: D**

VPC Service Controls creates a service perimeter that blocks data exfiltration at the network/API level regardless of valid IAM credentials, preventing copies to resources outside the approved perimeter — exactly the scenario described.

---

**Q42. Correct: B**

BigQuery data is always encrypted at rest by default; CMEK's distinguishing feature is that the customer holds and controls the key lifecycle in Cloud KMS, so revoking or destroying the key makes the encrypted data inaccessible, giving the customer an added layer of control the default option does not provide.

---

**Q43. Correct: B**

Row-level access policies let you define a filter predicate, evaluated per querying principal, that restricts which rows are returned from a shared table — enabling per-customer row filtering without duplicating tables or views.

---

**Q44. Correct: C**

BigQuery column-level security with Data Catalog policy tags lets you tag the sensitive column once and enforce dynamic data masking centrally based on IAM role, without duplicating data or creating extra views. Analysts querying the base table automatically see masked values (e.g., hashed/nulled), while compliance users with Fine-Grained Reader see the real value — all from the same source table, and the taxonomy scales across many tables.

---

**Q45. Correct: B**

Airflow's `sla` parameter flags a task as having missed its expected duration without killing it, and the `sla_miss_callback` plus a log-based metric wired to a Cloud Monitoring alerting policy gives automated, near-real-time (well within 15 minutes) notification — meeting the requirement to alert rather than terminate.

---

