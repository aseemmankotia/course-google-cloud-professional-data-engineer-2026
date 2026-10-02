# Practice Test 2: Google Cloud Professional Data Engineer Exam Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-2.md`. Track results in `score-tracker.md`.

---

**Q1.** An IoT platform ingests telemetry from 50 million devices into Cloud Bigtable using a row key of the form epoch_timestamp#device_id. Even though the cluster has 30 nodes, write throughput is bottlenecked on only a handful of nodes at any given moment. Which row key redesign BEST resolves this hotspotting while preserving the ability to query a device's recent readings efficiently?

A) Keep timestamp#device_id but append a random salt suffix to each key
B) Redesign the key as device_id#reverse_timestamp
C) Hash the entire epoch_timestamp value and use it as the row key
D) Split telemetry into separate column families per device

---

**Q2.** A global e-commerce company needs a single system of record for inventory that supports ACID transactions across multiple rows, SQL joins, horizontal write scalability as order volume grows, and 99.999% availability with multi-region writes. Which service should be selected?

A) Cloud Bigtable configured with a multi-cluster routing policy
B) Cloud SQL for PostgreSQL with cross-region read replicas
C) Cloud Spanner configured as a multi-region instance
D) BigQuery with multi-region datasets

---

**Q3.** A team of BigQuery analysts with strong SQL skills but no Python or Java experience needs to build version-controlled, testable ELT transformation pipelines that run entirely inside BigQuery, with explicit dependency graphs between transformation steps. Which tool should they use?

A) Cloud Data Fusion
B) Cloud Dataflow using SqlTransform
C) Dataform
D) Dataproc with PySpark SQL

---

**Q4.** A 50 TB BigQuery table holds five years of order history. Nearly every query filters on order_date, and most queries also filter or aggregate by customer_region and product_category. customer_id has extremely high cardinality. Which table design BEST optimizes cost and performance?

A) No partitioning; cluster by order_date, customer_region, product_category
B) Partition by order_date, cluster by customer_region, product_category
C) Partition by product_category, cluster by order_date
D) Partition by customer_id, cluster by order_date

---

**Q5.** A large enterprise has independent business units that each own and manage their own BigQuery datasets and GCS buckets under a data mesh model. Leadership wants centralized discovery, cataloging, data quality checks, and end-to-end lineage across all domains WITHOUT centralizing ownership or physically moving the data. Which GCP service should be used?

A) Cloud Asset Inventory
B) Dataplex
C) Cloud Data Catalog as a standalone service
D) Cloud Data Fusion

---

**Q6.** A company is migrating an on-premises Oracle database running heavy PL/SQL and sustaining 50,000 transactions per second. They want to minimize application rewrites by using a PostgreSQL-compatible interface, while requiring write throughput that scales horizontally beyond what a single primary instance can provide, with strong multi-row consistency. Which GCP service should they migrate to?

A) AlloyDB for PostgreSQL
B) Cloud SQL for PostgreSQL
C) BigQuery with the BigQuery migration Oracle connector
D) Cloud Spanner with the PostgreSQL interface

---

**Q7.** A fleet of 200,000 IoT devices streams sensor readings into Cloud Bigtable at roughly 50,000 writes/second. The team originally used the raw ingestion timestamp (epoch milliseconds) as the row key, and now writes are concentrated on a single node, creating a hotspot. The team also needs to efficiently scan the most recent readings for a specific device. Which row key design BEST solves the hotspotting problem while still supporting this query pattern?

A) Keep using the raw ingestion timestamp as the row key, but enable automatic load balancing on the Bigtable instance
B) Use SHA-256 hash of the timestamp as the row key so writes are evenly distributed across tablets
C) Use device_id concatenated with a reversed timestamp (e.g., Long.MAX_VALUE minus the timestamp) as the row key
D) Use device_id as the full row key and store all timestamped readings as cell values under a single column qualifier

---

**Q8.** A retail analytics team of SQL-only analysts needs to build a set of chained transformation queries entirely inside BigQuery, with dependency graph visualization, built-in data quality assertions, version control via Git, and scheduled execution — all without provisioning any compute clusters or writing Python/Java. Which GCP service should they use?

A) Cloud Data Fusion
B) Cloud Dataproc with Spark SQL jobs
C) Cloud Dataflow with SqlTransform
D) Dataform

---

**Q9.** A 50 TB BigQuery table ingests new rows daily. Query logs show: 90% of queries filter on order_date over the last 7-30 days, most of those queries also filter or GROUP BY on customer_id (5 million distinct values), and a smaller subset additionally filters on country (20 distinct values). You need to minimize bytes scanned and cost. Which table design is BEST?

A) Partition the table by order_date, and cluster by customer_id, then country
B) Partition the table by order_date, and cluster by country, then customer_id
C) Partition the table by customer_id, and cluster by order_date, then country
D) Do not partition the table; cluster only by order_date and customer_id

---

**Q10.** A global financial trading platform needs a database that provides multi-region synchronous replication, external strong consistency, full SQL with joins, multi-row ACID transactions spanning regions, and horizontal scalability to petabytes with 99.999% availability. Which GCP service should be used?

A) Cloud Bigtable
B) Firestore in Native mode
C) Cloud Spanner
D) Cloud SQL with cross-region read replicas

---

**Q11.** A streaming pipeline processes IoT sensor data using fixed 1-minute windows in Apache Beam on Dataflow. Some events arrive up to 10 minutes late due to unreliable field network connections. The team needs windows to emit an early result quickly, then update with a final corrected result once late data arrives, without firing on every single late element unnecessarily. Which configuration BEST meets this requirement?

A) Set allowed lateness to 10 minutes and configure early and late firings using triggers with accumulating mode
B) Use session windows instead of fixed windows to capture late data automatically
C) Increase window size to 10 minutes to include all late data
D) Use a global window with a count-based trigger

---

**Q12.** A pipeline reads from Pub/Sub, processes records in Dataflow, and writes streaming inserts to a BigQuery table. The team must guarantee that no record is processed or written more than once end-to-end, even if workers restart or retries occur. Which statement correctly describes how to achieve this?

A) Pub/Sub subscription's at-least-once delivery already guarantees exactly-once end-to-end
B) Configure the subscription for at-most-once delivery to avoid duplicates
C) Implement manual deduplication using Cloud Memorystore before publishing to Pub/Sub
D) Rely on Dataflow's exactly-once processing model combined with BigQueryIO's streaming insert deduplication using insertId for effectively-once end-to-end delivery

---

**Q13.** A company is migrating an on-premises Oracle database to BigQuery and needs continuous low-latency replication of inserts, updates, and deletes with minimal load placed on the source database. Which approach is the current recommended solution using Google Cloud managed CDC tooling?

A) Stream CDC events to Cloud Storage as Avro files, then run a Dataflow batch job every hour to merge into BigQuery
B) Configure Datastream with BigQuery as the direct destination, letting Datastream handle continuous replication and merge operations automatically
C) Use Data Fusion to poll the Oracle database every 5 minutes and perform batch loads into BigQuery
D) Deploy a Dataproc Spark job using a JDBC connector to extract the full table nightly

---

**Q14.** A team is migrating on-premises Spark ETL jobs that each run for about 30 minutes, several times per day, with idle time between runs. They currently use a persistent Dataproc cluster and want to minimize cost without managing cluster lifecycle logic. What should they do?

A) Configure Dataproc Autoscaling Policies on the existing persistent cluster with a minimum of 0 secondary workers
B) Migrate the jobs to Dataproc Serverless for Spark, which runs each job as an ephemeral batch with no cluster to manage or pay for when idle
C) Use a preemptible-only cluster with autoscaling disabled to minimize compute cost
D) Increase the size of the persistent cluster so each job finishes faster, reducing total billed time

---

**Q15.** A Cloud Composer DAG has a downstream task that must run only after a file lands in a Cloud Storage bucket, but the file arrival time is unpredictable and can occur at any point during the day. The team wants to minimize wasted compute and worker-slot usage while reacting quickly to file arrival. What is the BEST design?

A) Configure a Cloud Storage notification to publish to Pub/Sub, and use a Cloud Function to call the Airflow REST API and trigger the DAG run when the object is created
B) Use a GCSObjectExistenceSensor in poke mode, polling every 30 seconds for the duration of the DAG
C) Schedule the DAG hourly and rely on task retries until the file appears
D) Use a deferrable GCSObjectExistenceSensor in reschedule mode polling every 60 seconds

---

**Q16.** A streaming Dataflow pipeline ingests JSON events from Pub/Sub. Occasionally, upstream producers send malformed or schema-drifted records that cause exceptions in the parsing DoFn, and currently these exceptions cause bundle-level failures that stall the pipeline. The team needs the pipeline to keep processing valid records while capturing failed records for later inspection. What should they implement?

A) Wrap the transform logic in a try/catch inside the DoFn, routing malformed records to a dead-letter PCollection written to a BigQuery error table or GCS while valid records continue downstream
B) Configure the Pub/Sub subscription to silently discard any message that fails JSON parsing before it reaches Dataflow
C) Rely on Dataflow's built-in retry with exponential backoff, which will eventually parse the malformed records correctly
D) Increase the number of Dataflow workers so malformed elements are processed through brute-force parallelism

---

**Q17.** A streaming Dataflow pipeline aggregates IoT sensor readings using fixed 5-minute windows. Sensors occasionally lose connectivity and send readings up to 2 hours late. The team wants late readings incorporated into window results, but does not want to increase the base window size (which would delay all results) or trigger a full pipeline reprocess for each late arrival. Which configuration BEST meets this requirement?

A) Change the fixed window duration to 2 hours so no data arrives late
B) Set withAllowedLateness(Duration.standardHours(2)) on the windowing strategy and use a trigger with accumulating-fired panes to emit updated window results as late data arrives
C) Switch to a single global window with a count-based trigger to avoid watermark issues entirely
D) Use session windows with a 2-hour gap duration instead of fixed windows

---

**Q18.** A company needs to replicate change data from an on-premises MySQL database to BigQuery with low latency, automatic handling of schema changes, and minimal operational overhead. Which approach BEST satisfies these requirements?

A) Write a custom Dataflow pipeline that performs full JDBC batch reads from MySQL on a fixed schedule
B) Use Datastream to capture change data from MySQL and the Datastream-to-BigQuery Dataflow template to load changes continuously
C) Run a nightly Dataproc Spark job that queries MySQL via JDBC and overwrites the BigQuery table
D) Build a custom producer that polls the MySQL binlog and publishes rows to Pub/Sub for a downstream subscriber

---

**Q19.** A team is lifting and shifting an existing on-premises Spark ETL job that runs for about 2 hours nightly. They want to minimize cluster management overhead and cost, without rewriting the job in another framework. Which Dataproc-based approach BEST fits these requirements?

A) Provision a single long-running Dataproc cluster sized for peak load and keep it running continuously
B) Deploy Dataproc on GKE with manually managed node pools sized to the largest historical job
C) Submit the job as a Dataproc Serverless for Spark batch workload, which provisions resources automatically per job and scales without persistent infrastructure
D) Rewrite the Spark job in Apache Beam and run it on Dataflow instead of Dataproc

---

**Q20.** A Dataflow streaming pipeline reads payment transaction messages from a Pub/Sub subscription and writes results to BigQuery. Despite Dataflow's internal exactly-once processing guarantee, the finance team occasionally sees duplicate transaction rows in BigQuery, causing double-counted revenue. Which change BEST eliminates the duplication end-to-end?

A) Increase the number of Dataflow workers so messages are processed faster and duplicates have less time to occur
B) Switch the Pub/Sub subscription from push to pull with a longer acknowledgment deadline
C) Insert a Cloud Function between Pub/Sub and Dataflow to pre-filter duplicate messages before they reach the pipeline
D) Enable exactly-once delivery on the Pub/Sub subscription and configure PubsubIO to use a deterministic per-message ID for deduplication before writing to BigQuery

---

**Q21.** A pipeline uses Datastream to capture changes from a PostgreSQL source and the Datastream-to-BigQuery Dataflow template to load them into BigQuery. The source team adds a new nullable column to a replicated table without prior notice. Which outcome should the data engineer expect, and what action, if any, is required?

A) The pipeline will fail immediately and the stream must be manually recreated from scratch to pick up the new column
B) The Dataflow template must be manually redeployed with an updated schema definition before replication can continue
C) Datastream detects the additive schema change and the BigQuery destination table is automatically updated with the new nullable column; no manual intervention is required
D) The new column will be silently dropped during replication and must be backfilled later using a separate Dataform transformation

---

**Q22.** A company ingests IoT sensor readings into Bigtable using a row key composed solely of a Unix timestamp (e.g., '1706745600'). During peak ingestion, most writes land on a single tablet server, causing severe hotspotting even though the cluster has many nodes. Which row key redesign BEST resolves this?

A) Reverse the digits of the timestamp before writing the row key
B) Prefix the row key with a hash of the sensor ID, followed by the timestamp (e.g., hash(sensor_id)#timestamp)
C) Add more nodes to the Bigtable cluster to spread the load
D) Append a monotonically increasing sequence number after the timestamp

---

**Q23.** A 50TB BigQuery fact table 'orders' grows daily. Nearly every analytical query filters on order_date over a rolling 90-day range and filters on customer_region (medium cardinality, ~50 values); a smaller subset of queries also filters on product_category (high cardinality) within that region/date scope. Which table design BEST optimizes cost and performance for this access pattern?

A) Partition on customer_region; cluster on order_date
B) Partition on order_date; cluster on product_category, then customer_region
C) Partition on order_date; cluster on customer_region, then product_category
D) Cluster on order_date; partition on customer_region

---

**Q24.** A media company stores web server logs in Cloud Storage. Access patterns are unpredictable: some log files are queried heavily for weeks, others go cold within days. Logs must be retained for 7 years for compliance, and legal requires that retained objects cannot be deleted or overwritten early, even by an administrator. Which configuration BEST meets these requirements while minimizing storage cost?

A) Set the default storage class to Coldline for all incoming objects
B) Use Nearline as the default class with a lifecycle rule to move objects to Archive after 30 days
C) Write manual lifecycle rules that transition objects to Coldline at 30 days and Archive at 365 days based on object age
D) Enable Autoclass on the bucket and configure a Bucket Lock retention policy

---

**Q25.** You are designing the backend for a global multiplayer game that must maintain strongly consistent, externally consistent player-state transactions across multiple continents, support complex relational joins, and scale horizontally to petabytes with a 99.999% availability SLA. Which storage service should you choose?

A) Cloud Bigtable
B) Cloud Spanner with a multi-region instance configuration
C) Cloud SQL for PostgreSQL with cross-region read replicas
D) BigQuery with a multi-region dataset

---

**Q26.** A dataset's time travel window is configured at the default 7 days. A critical production table was accidentally dropped 10 days ago and nobody noticed until now. What is the correct statement about recovering this table's data?

A) The data may still be recoverable by contacting Google Cloud Support during the fail-safe period, but it cannot be queried directly with standard time travel SQL
B) The table data is permanently and irrecoverably deleted the moment the 7-day time travel window expires
C) You can still run a query using FOR SYSTEM_TIME AS OF against the dropped table to recover the data
D) BigQuery automatically restores dropped tables from a nightly scheduled snapshot after 7 days

---

**Q27.** A retail analytics team runs BigQuery workloads that are nearly idle most months but spike to require massive concurrent slot capacity for 3-4 days during quarterly batch reprocessing. Finance wants to avoid a long-term annual commitment while still guaranteeing capacity during the spikes. Which approach BEST fits these requirements?

A) Purchase short-term flex-rate slot commitments (via BigQuery editions/reservations) only during the spike periods, with autoscaling enabled
B) Rely solely on on-demand pricing year-round with no reservations
C) Purchase an annual flat-rate slot commitment sized for the quarterly peak
D) Purchase a rolling monthly flat-rate commitment sized for the quarterly peak, active year-round

---

**Q28.** A fleet of 50,000 IoT sensors sends one row per reading to Bigtable, roughly 10 writes/sec per sensor. Downstream analytics need to scan all readings for a single sensor in reverse-chronological order (most recent first). The current row key is a plain Unix timestamp in milliseconds, and the cluster is showing severe hotspotting on a small number of nodes during peak load. Which row key design BEST fixes the hotspotting while preserving efficient per-sensor, most-recent-first range scans?

A) Concatenate sensor_id followed by (Long.MAX_VALUE - timestamp_ms), e.g. <sensor_id>#<reverse_timestamp>
B) Prefix the row key with a random 2-digit salt, e.g. <random_salt>#<sensor_id>#<timestamp>
C) Use an auto-incrementing integer ID generated by a centralized counter as the row key
D) Keep the timestamp as the row key but increase the number of Bigtable nodes to absorb the hotspot

---

**Q29.** A dataset's tables are frequently subject to accidental DML deletes. The data team wants to guarantee at least 2 days of point-in-time recovery via BigQuery time travel while minimizing the extra storage cost billed for time-travel-retained bytes, and they understand fail-safe storage is a separate, non-configurable safety net. What should they do?

A) Run ALTER DATASET SET OPTIONS(max_time_travel_hours = 48) on the dataset
B) Take a manual table snapshot every day and delete tables immediately after each snapshot completes
C) Do nothing, because BigQuery time travel is fixed at 7 days for every dataset and cannot be shortened
D) Disable time travel entirely at the project level using the bq CLI --disable_time_travel flag

---

**Q30.** A global order-processing system needs strongly consistent, multi-row ACID transactions across a Orders table and a nested OrderItems table with a parent-child relationship, SQL joins for reporting, automatic horizontal scaling beyond what a single regional instance can handle, and synchronous multi-region replication for 99.999% availability. Which storage design BEST meets all these requirements?

A) Cloud SQL for PostgreSQL with cross-region read replicas and application-level sharding
B) Bigtable with OrderItems denormalized into the Orders row using column qualifiers as item IDs
C) Firestore in Datastore mode with Orders and OrderItems as separate top-level collections
D) Cloud Spanner with OrderItems interleaved inside the Orders parent table

---

**Q31.** A retail company wants to build an image classification model to detect defective products from photos taken on an assembly line. They have 500,000 labeled images stored in Cloud Storage. Which approach BEST meets this requirement?

A) Use BigQuery ML CREATE MODEL with model_type='automl_classifier', referencing the image URIs directly in a BigQuery table
B) Use Vertex AI AutoML Vision (or a custom training job with a pretrained CNN architecture), then deploy the resulting model to a Vertex AI endpoint
C) Load the images as BYTES columns into BigQuery and train a BigQuery ML BOOSTED_TREE_CLASSIFIER on the raw pixel bytes
D) Use a BigQuery ML TRANSFORM clause with ML.STANDARD_SCALER to preprocess pixel values, then train a logistic regression model

---

**Q32.** A data engineer creates a BigQuery ML model with the following statement:

CREATE MODEL mydataset.my_model
TRANSFORM(
  ML.STANDARD_SCALER(feature1) OVER() AS scaled_feature1,
  label
)
OPTIONS(model_type='linear_reg', input_label_cols=['label']) AS
SELECT feature1, label FROM mydataset.training_table;

What is the primary benefit of using the TRANSFORM clause here?

A) It reduces training time by pre-aggregating rows before scaling is applied
B) It silently converts the model to a boosted tree model type for faster inference at prediction time
C) It exports the trained model directly into Vertex AI Feature Store for low-latency online serving
D) It automatically applies the identical feature preprocessing at prediction time as was used during training, without requiring the analyst to reapply the transformation manually

---

**Q33.** A machine learning team trains fraud-detection models in Vertex AI using features computed daily in BigQuery (for example, rolling 7-day transaction counts per user). At serving time, an online fraud-scoring API needs these same feature values with p99 latency under 10 milliseconds. Which solution should they implement?

A) Query BigQuery directly from the serving API and rely on BigQuery's query result cache for repeated lookups
B) Enable BigQuery BI Engine reserved capacity on the features table to accelerate reads for the serving API
C) Use Vertex AI Feature Store to sync the batch-computed features from BigQuery into an online store optimized for low-latency key-based lookups
D) Export the features to Cloud Storage as Avro files and have the serving application read the relevant file on each request

---

**Q34.** A healthcare analytics company wants to share a curated, continuously updated BigQuery dataset with 12 external hospital partners so each partner can run their own queries, without giving them direct access to the source GCP project or physically copying the data into each partner's project. Which approach is correct?

A) Publish the dataset as an Analytics Hub listing and have each hospital subscribe, creating a linked dataset in their own project
B) Grant each hospital's service account the BigQuery Data Viewer IAM role directly on the source project
C) Export the dataset nightly to Cloud Storage as CSV files and share time-limited signed URLs with each hospital
D) Use BigQuery Data Transfer Service to copy the dataset into each partner's project on a recurring schedule

---

**Q35.** Which statement about the BigQuery ML ARIMA_PLUS model type is correct?

A) It requires the analyst to manually specify seasonal periods and Fourier terms before training can begin
B) It only supports forecasting a single time series per CREATE MODEL statement
C) It has been deprecated in favor of BOOSTED_TREE_REGRESSOR for all time-series forecasting tasks
D) It automatically detects seasonality, handles missing or irregular values, and can incorporate holiday effects without manual feature engineering

---

**Q36.** An executive dashboard built in Looker Studio, connected directly to a BigQuery table via a live connection, consistently shows data that is 12 hours stale, even though the underlying table is updated every 15 minutes by a Dataflow pipeline. Viewers are not manually refreshing the page. What is the MOST likely cause?

A) BigQuery streaming inserts have a default 12-hour visibility delay before they appear in query results
B) Dataflow pipelines are restricted from writing to BigQuery tables that are actively queried by BI tools within the same 24-hour window
C) The connected table is a materialized view, and materialized views can only refresh once per day by default
D) The Looker Studio report's data freshness (cache) setting is configured to refresh cached query results only every 12 hours

---

**Q37.** A retail company built a BigQuery ML logistic regression model using `CREATE MODEL ... OPTIONS(model_type='logistic_reg')`. Business requirements now demand that a mobile checkout app call the model for fraud-risk scoring with sub-100ms p99 latency per request. The team is currently calling `ML.PREDICT` from a scheduled query and writing results to a table, which introduces minutes of staleness. What should they do to meet the new latency requirement while reusing the existing trained model?

A) Run `EXPORT MODEL` to write the model to Cloud Storage in TensorFlow SavedModel format, import it into Vertex AI Model Registry, and deploy it to a Vertex AI online prediction endpoint
B) Increase the BigQuery reservation slots assigned to the project so ML.PREDICT queries return faster for each request
C) Enable BigQuery BI Engine on the dataset so that ML.PREDICT queries are served from an in-memory cache with millisecond latency
D) Retrain the model from scratch in Vertex AI AutoML Tables using the same training table, then deploy the AutoML model to an endpoint

---

**Q38.** A Cloud Composer environment hosts an hourly DAG with catchup=True (the Airflow default). The environment underwent unplanned maintenance and was offline for 30 hours. On restart, the engineer wants Airflow to execute only the single most recent scheduled interval, not all 30 missed intervals. Which change achieves this with minimal side effects?

A) Set max_active_runs=1 on the DAG
B) Pause the DAG, then immediately unpause it after the environment restarts
C) Set the DAG's catchup parameter to False
D) Change schedule_interval to None for that DAG

---

**Q39.** A streaming Dataflow pipeline reads from Pub/Sub, performs windowed aggregations, and writes to BigQuery. Engineers need an early warning when the pipeline is falling behind incoming events, before downstream dashboards show stale results. Which Cloud Monitoring metric should the alerting policy be based on?

A) Worker CPU utilization
B) Current number of autoscaled workers
C) Total bytes written to BigQuery sinks
D) Data freshness (job/data_watermark_age)

---

**Q40.** A company's BigQuery dataset is created in the single region us-central1. Compliance requires that in a regional outage the dataset can be restored elsewhere with a recovery point objective (RPO) of 24 hours. What should be implemented?

A) Configure a scheduled BigQuery Data Transfer Service copy job that replicates the dataset to a dataset in a different region on a daily schedule
B) Rely on BigQuery's automatic storage replication since all BigQuery datasets are inherently multi-region
C) Take nightly Persistent Disk snapshots of the BigQuery storage layer
D) Enable Cloud Storage Nearline as the default storage class for the dataset

---

**Q41.** Data analysts need to author and run BigQuery scheduled queries that write results into a destination dataset, but must not be able to delete the dataset or modify its IAM policy. Which role assignment best satisfies least privilege?

A) roles/owner on the project
B) roles/bigquery.dataOwner on the destination dataset plus roles/bigquery.admin on the project
C) roles/bigquery.admin on the project
D) roles/bigquery.dataEditor on the destination dataset plus roles/bigquery.jobUser on the project

---

**Q42.** A Dataflow job runs inside a VPC Service Controls perimeter that also protects several BigQuery datasets. The job must consume messages from a Pub/Sub topic that lives in a separate project not included in the perimeter, and that project cannot be added to the perimeter for organizational reasons. How should access be granted while keeping the perimeter's protections intact?

A) Configure an egress rule on the perimeter that explicitly allows the Dataflow service account to access the specific external Pub/Sub resource
B) Disable VPC Service Controls enforcement mode for the perimeter until the job completes
C) Grant the Dataflow service account project-level Editor on the external project and rely on IAM alone
D) Enable Private Google Access on the Dataflow subnet

---

**Q43.** A BigQuery table contains a national ID column. Regular analysts should see the ID with only the last 4 digits exposed, while the compliance team must see the full unmasked value for audits. Both groups need to query the same live table without maintaining separate copies. Which approach meets this requirement?

A) Run a Cloud DLP de-identification job once during ingestion to permanently mask the column for all readers
B) Apply row-level security policies that filter rows based on group membership
C) Apply a Data Catalog policy tag with a masking rule to the column, grant analysts the Masked Reader role, and grant compliance the Fine-Grained Reader role on that policy tag
D) Create two authorized views on the table, one with the column removed and one with the column included, and grant access to each view separately

---

**Q44.** A healthcare company stores PHI in BigQuery. Security requires that this data can never be exfiltrated to any resource outside the organization's security perimeter, even by an attacker using a stolen OAuth token from a legitimate analyst account. At the same time, an on-premises analytics appliance connected via Cloud Interconnect must be able to query the BigQuery data directly using private connectivity. Which approach BEST satisfies both requirements?

A) Enable Customer-Managed Encryption Keys (CMEK) on all BigQuery datasets and restrict Cloud KMS key usage through IAM, relying on encryption to prevent unauthorized export of data.
B) Grant the on-prem analytics service account the roles/bigquery.dataViewer role at the project level and enable Cloud Audit Logs with a log-based alert to detect exfiltration after it occurs.
C) Create a VPC Service Controls perimeter containing the BigQuery project, and define an ingress policy (via Access Context Manager) that permits access only from the on-prem network's identity and IP range, connected through a Private Service Connect endpoint over Cloud Interconnect.
D) Place the on-prem analytics tool's proxy inside a GKE cluster in the same VPC as the BigQuery jobs, and use VPC firewall rules to restrict egress traffic to approved external IP ranges.

---

**Q45.** A retailer's BigQuery `customers` table has a `national_id` column subject to regional privacy law. Requirements: (1) EU-based analysts may see plaintext `national_id` only for EU customer rows, (2) analysts outside the EU must always see a masked value for that column regardless of customer region, and (3) all analysts must still be able to run aggregate queries (e.g., COUNT) against the column without errors. Which design meets all three requirements with the least ongoing maintenance?

A) Apply AEAD encryption functions to the national_id column at load time, storing ciphertext, and grant Cloud KMS DECRYPT permission on the wrapping key only to EU analysts' service accounts.
B) Create a row access policy on the table that limits EU analysts to EU customer rows using SESSION_USER(), and apply a Data Catalog policy tag with a default masking rule on national_id so only principals granted the Fine-Grained Reader role on that tag see plaintext; all others automatically receive a masked value.
C) Maintain two authorized views: one selecting the raw national_id column for EU analysts and one omitting the column entirely for all other analysts, granting SELECT on the appropriate view per IAM group.
D) Grant every analyst the BigQuery Fine-Grained Reader role globally, and run a nightly scheduled query that overwrites national_id with a redacted value for non-EU customer rows.

---

