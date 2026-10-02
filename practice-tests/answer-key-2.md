# Answer Key — Practice Test 2

**Q1. Correct: B**

Promoting the high-cardinality device_id to the front of the key spreads writes across the full tablet key range instead of all writes landing near the same monotonically-increasing timestamp prefix. Reversing the timestamp still lets you scan a device's most recent readings first with a simple prefix scan.

---

**Q2. Correct: C**

Spanner is the only GCP database offering horizontally scalable, strongly consistent multi-row ACID transactions with SQL and a documented 99.999% multi-region SLA, making it the correct fit for global transactional inventory.

---

**Q3. Correct: C**

Dataform is purpose-built for SQL-based ELT inside BigQuery: it provides dependency (DAG) management, version control integration, and built-in assertions/tests for SQL transformations without requiring general-purpose programming skills.

---

**Q4. Correct: B**

Date partitioning lets BigQuery prune entire partitions for the near-universal order_date filter, and clustering on the two next-most-common filter columns further prunes blocks within each partition, minimizing bytes scanned.

---

**Q5. Correct: B**

Dataplex provides a unified governance layer (catalog, data quality, lineage) that spans distributed BigQuery and GCS assets across business domains while leaving the underlying data in place and owned by its respective teams, which is exactly the data mesh governance pattern.

---

**Q6. Correct: D**

Spanner's PostgreSQL interface offers SQL/driver compatibility to ease migration while providing true horizontal write scalability across nodes with strong multi-row consistency, which is required at 50,000 TPS beyond any single-instance limit.

---

**Q7. Correct: C**

Prefixing with device_id spreads writes across many row-key ranges (one per device), eliminating the hotspot, while the reversed timestamp keeps each device's readings sorted with the most recent value first, making 'get latest readings for device X' an efficient prefix scan.

---

**Q8. Correct: D**

Dataform is purpose-built for SQL-based ELT inside BigQuery: it manages dependency graphs (DAGs) of SQL transformations, supports Git-based version control, includes declarative data quality assertions, and requires zero cluster or pipeline infrastructure to manage.

---

**Q9. Correct: A**

Date-range partitioning matches the dominant date-range filter and lets BigQuery prune whole partitions. Clustering column order should reflect filter/GROUP BY frequency, so customer_id (used in nearly every query) goes first, with the lower-frequency country filter second — this maximizes block pruning within each partition.

---

**Q10. Correct: C**

Cloud Spanner is the only option offering horizontally scalable relational storage with full SQL, multi-row ACID transactions, external strong consistency via TrueTime, and synchronous multi-region replication with up to 99.999% availability SLA at petabyte scale.

---

**Q11. Correct: A**

Allowed lateness (withAllowedLateness) extends the window's data-driven trigger horizon so late data is still accepted and merged into the pane. Combining this with an early trigger (e.g., processing-time trigger for a fast provisional result) and a late-firing trigger produces controlled emission of an early estimate followed by a corrected final result, using accumulating mode so later panes include all prior data rather than just deltas.

---

**Q12. Correct: D**

Dataflow's runner guarantees exactly-once processing of records within the pipeline itself (it dedupes retried bundles internally). For the final sink write, BigQueryIO's streaming insert API uses an insertId per row so that BigQuery's dedup logic on the streaming buffer suppresses duplicate rows from retried writes, achieving effectively-once semantics end-to-end.

---

**Q13. Correct: B**

Datastream supports BigQuery as a direct replication destination, natively capturing change events (inserts/updates/deletes) from Oracle's redo logs with minimal source impact and automatically applying merges into BigQuery tables, removing the need for a separate Dataflow merge pipeline for the common case.

---

**Q14. Correct: B**

Dataproc Serverless for Spark eliminates cluster provisioning and management entirely — Google runs each batch job on transient infrastructure and bills only for the resources consumed during execution, which is ideal for short, periodic jobs with idle gaps and requires no autoscaling policy tuning.

---

**Q15. Correct: A**

Decoupling the trigger from any polling mechanism by using a GCS event notification through Pub/Sub and a Cloud Function calling the Airflow REST API creates a true event-driven trigger, minimizing latency between file arrival and DAG execution while consuming zero compute while waiting.

---

**Q16. Correct: A**

Catching exceptions inside the DoFn and using a multi-output ParDo (TupleTags) to split successful and failed elements into separate PCollections is the standard dead-letter pattern in Beam/Dataflow. It isolates malformed records for review or reprocessing without stalling the main pipeline, directly addressing schema drift robustness.

---

**Q17. Correct: B**

withAllowedLateness extends the window's lifetime so late data within that bound is still accepted, and accumulating mode with a repeated/late trigger allows the pipeline to re-fire the window pane with updated results when late data arrives, without changing the base window size or reprocessing the whole pipeline.

---

**Q18. Correct: B**

Datastream is purpose-built for CDC from MySQL/Oracle/PostgreSQL sources, and combined with the managed Datastream-to-BigQuery Dataflow template, it delivers low-latency streaming replication with automatic schema drift handling and minimal ops.

---

**Q19. Correct: C**

Dataproc Serverless for Spark runs Spark batch workloads without requiring you to provision or manage a cluster; it autoscales per job and only bills for actual usage, which is ideal for a short nightly job while preserving existing Spark code unchanged.

---

**Q20. Correct: D**

Dataflow's internal exactly-once processing does not guarantee end-to-end exactly-once delivery if the source delivers duplicates and the sink doesn't dedupe on retries; enabling Pub/Sub's exactly-once delivery combined with a deterministic message ID for PubsubIO-level deduplication closes this gap so duplicates from redelivery are correctly filtered before the BigQuery write.

---

**Q21. Correct: C**

Datastream and the Datastream-to-BigQuery template are designed to handle backward-compatible schema drift such as adding a new nullable column automatically, updating the BigQuery destination schema without requiring the stream to be recreated or the pipeline to be manually redeployed.

---

**Q22. Correct: B**

Hashing (or otherwise distributing) a high-cardinality field like sensor_id and placing it before the timestamp spreads writes across the key space and thus across tablets, eliminating the hot tablet caused by monotonically increasing timestamp keys. This is the standard Bigtable anti-hotspotting pattern tested on the exam.

---

**Q23. Correct: C**

Partitioning by order_date lets BigQuery prune partitions outside the 90-day window before scanning, which every query benefits from. Clustering by customer_region first (used by nearly all queries) and then product_category (used by a subset) further sorts blocks so both filters can prune data, maximizing bytes-scanned savings.

---

**Q24. Correct: D**

Autoclass automatically moves individual objects between Standard, Nearline, Coldline, and Archive based on actual access patterns rather than a fixed age, which is ideal when access is unpredictable — it saves cost without manual tuning. Bucket Lock enforces an immutable retention policy so objects genuinely cannot be deleted or overwritten before the 7-year period, satisfying the legal requirement — something plain lifecycle rules or storage class choice cannot do.

---

**Q25. Correct: B**

Cloud Spanner is Google's globally distributed, horizontally scalable relational database offering external consistency (true serializability) across regions, SQL joins, and up to 99.999% availability with a multi-region configuration — exactly matching all stated requirements.

---

**Q26. Correct: A**

After the configurable time travel window (default 7 days) expires, BigQuery enters an additional fail-safe period (up to 7 more days) during which deleted/changed data is not queryable via SQL but can still potentially be recovered by engaging Google Cloud Support — this is a specific, exam-tested nuance beyond plain time travel.

---

**Q27. Correct: A**

Flex-rate / short-term slot commitments (available through BigQuery editions and reservations) can be purchased for as little as 60 seconds up to a defined duration, letting you guarantee slot capacity exactly during the multi-day spike and cancel afterward — avoiding both unpredictable on-demand cost spikes and long-term (annual/monthly) commitment waste. Autoscaling reservations further let capacity flex within that window.

---

**Q28. Correct: A**

Prefixing with sensor_id spreads writes across the key space because 50,000 distinct sensor IDs are naturally well distributed, eliminating hotspotting on any single tablet. Subtracting the timestamp from a large constant (reverse timestamp) keeps lexicographic ordering equal to reverse-chronological order, so a scan bounded by a single sensor_id prefix returns the most recent readings first without a full table scan.

---

**Q29. Correct: A**

The max_time_travel_hours dataset option can be set to any value between 48 and 168 hours (2 to 7 days). Setting it to 48 satisfies the 2-day recovery requirement while minimizing the window of extra storage billed for changed/deleted bytes retained for time travel, compared to the 7-day default. The separate 7-day fail-safe period still applies afterward regardless of this setting, as a Support-only recovery mechanism.

---

**Q30. Correct: D**

Cloud Spanner natively supports interleaved child tables co-located with their parent rows, giving locality for efficient joins, strong external consistency, multi-row ACID transactions, standard SQL, and transparent horizontal scaling with synchronous multi-region replication reaching up to 99.999% availability in multi-region configs — matching every stated requirement.

---

**Q31. Correct: B**

Vertex AI natively supports image classification through AutoML Vision or custom training jobs using pretrained CNN architectures, which is the correct tool for complex unstructured image workloads at this scale.

---

**Q32. Correct: D**

The TRANSFORM clause captures preprocessing logic inside the model definition, so ML.PREDICT automatically re-applies the exact same transformation to new input data, guaranteeing training/serving consistency and reproducibility.

---

**Q33. Correct: C**

Vertex AI Feature Store is purpose-built to bridge batch-computed offline features (e.g., from BigQuery) with a low-latency online serving store, enabling millisecond-level key lookups required for real-time inference.

---

**Q34. Correct: A**

Analytics Hub is designed for exactly this scenario: a publisher exposes a listing, and subscribers create read-only linked datasets in their own projects that stay continuously in sync with the source data, without direct project access or data duplication.

---

**Q35. Correct: D**

ARIMA_PLUS automatically detects seasonal patterns, handles missing and irregular time points, performs anomaly detection, and can adjust for holiday effects via the holiday_region option, minimizing manual feature engineering.

---

**Q36. Correct: D**

Looker Studio caches query results according to a configurable data freshness setting at the data source or chart level; if that setting is configured for a 12-hour interval, viewers will see stale data regardless of how frequently the underlying BigQuery table updates.

---

**Q37. Correct: A**

BigQuery ML models can be exported with the `EXPORT MODEL model_name OPTIONS(URI='gs://...')` statement into TensorFlow SavedModel format (for supported model types like logistic_reg, linear_reg, boosted trees, DNN, k-means). That artifact can be imported into Vertex AI Model Registry and deployed to an online prediction endpoint, giving low-latency, per-request REST/gRPC serving that ML.PREDICT (a SQL/batch execution engine) cannot provide, while reusing the already-trained model.

---

**Q38. Correct: C**

catchup is a per-DAG setting that controls whether the scheduler backfills every missed schedule interval since the last successful run. Setting catchup=False tells the scheduler to skip missed intervals and only create a DAG run for the most recent schedule, exactly matching the requirement.

---

**Q39. Correct: D**

Data freshness (job/data_watermark_age) measures the difference between the pipeline's output watermark and real time. A growing value directly indicates the pipeline cannot keep pace with incoming events, which is exactly the 'falling behind' condition being monitored.

---

**Q40. Correct: A**

A dataset created in a single region (us-central1) is not automatically replicated across regions. Scheduling a BigQuery Data Transfer Service dataset copy job on a daily cadence to a dataset in another region gives a concrete, verifiable RPO of 24 hours and a target location to restore from during an outage.

---

**Q41. Correct: D**

roles/bigquery.dataEditor on the destination dataset grants the table/data-level permissions needed to write scheduled query results, while roles/bigquery.jobUser at the project level allows running query jobs. Neither role grants dataset-level setIamPolicy or dataset-delete permissions, satisfying least privilege.

---

**Q42. Correct: A**

VPC Service Controls supports egress rules that identify a specific identity (the Dataflow service account) and a specific external resource (the Pub/Sub topic/project) that is permitted to be accessed from inside the perimeter, without adding the whole external project to the perimeter or disabling protection.

---

**Q43. Correct: C**

BigQuery column-level security with Data Catalog policy tags supports dynamic data masking: the same underlying column is masked at query time for principals with the Masked Reader role, while principals granted Fine-Grained Reader (or Data Policy admin equivalent unmasked access) see the full value -- all against one live table with no duplication.

---

**Q44. Correct: C**

VPC Service Controls creates a service perimeter around BigQuery so that even a valid, stolen credential cannot be used to move data to a resource outside the perimeter. Combining this with an ingress policy scoped to a specific identity and network path (Private Service Connect over Interconnect) allows the on-prem tool controlled private access while blocking exfiltration to any other destination, including other GCP projects or the public internet.

---

**Q45. Correct: B**

BigQuery row-level security (row access policies keyed on SESSION_USER()/group membership) restricts which rows an analyst sees, while column-level security via Data Catalog policy tags with a masking rule dynamically returns masked values to anyone without the Fine-Grained Reader role on that tag -- and unlike dropping the column, dynamic masking still returns a value so COUNT() and other aggregates work normally. This combination requires no per-analyst view duplication and enforces both region- and role-based access at query time.

---

