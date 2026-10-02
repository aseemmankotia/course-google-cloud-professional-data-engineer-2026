# Practice Test 1: Google Cloud Professional Data Engineer Exam Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-1.md`. Track results in `score-tracker.md`.

---

**Q1.** A logistics company ingests telemetry from 2 million IoT sensors, sustaining 500,000 writes per second. Application dashboards require single-digit millisecond p99 read latency on individual sensor keys, and the dataset will grow to multiple petabytes with a flexible wide-column schema. Which storage service BEST meets these requirements?

A) Cloud SQL for PostgreSQL
B) Firestore in Native mode
C) Cloud Bigtable
D) BigQuery

---

**Q2.** A payments company must flag potentially fraudulent transactions within 2 seconds of occurrence. Transactions arrive continuously and unbounded, and the solution must support exactly-once processing with windowed aggregations over sliding time windows. Which architecture BEST satisfies these requirements?

A) A Dataflow streaming pipeline consuming from Pub/Sub with windowed transforms
B) A Cloud Composer DAG that batches transactions hourly
C) A BigQuery scheduled query running every 5 minutes
D) A Dataproc Spark batch job triggered hourly

---

**Q3.** A 50 TB BigQuery orders table is queried heavily with WHERE clauses filtering on order_date over the last 30 days and equality filters on customer_id. Which table design minimizes bytes scanned and query cost?

A) Partition the table by ingestion time only, no clustering
B) Partition the table by order_date and cluster by customer_id
C) Cluster the table by order_date, do not partition
D) Denormalize into one wide table partitioned by customer_id

---

**Q4.** A team has an existing Apache Spark codebase with custom MLlib feature engineering jobs running on-premises Hadoop/HDFS. They want to migrate to GCP with minimal code rewrite while retaining full Spark and MLlib compatibility. Which service should they choose?

A) Dataproc
B) Dataflow
C) Cloud Data Fusion
D) Dataform

---

**Q5.** A global retail application requires strongly consistent relational transactions, horizontal write scalability across multiple regions, native SQL support, and 99.999% availability. Which database should be selected?

A) Cloud SQL with cross-region read replicas
B) BigQuery
C) Cloud Bigtable
D) Cloud Spanner configured as a multi-region instance

---

**Q6.** An organization wants centralized discovery, cataloging, and automated lineage tracking across BigQuery datasets, Cloud Storage buckets, and Pub/Sub topics, with fine-grained access policies attached to logical business data domains as part of a data mesh strategy. Which service should be the foundation of this design?

A) Cloud DLP
B) Cloud Composer
C) Standalone Data Catalog (legacy)
D) Dataplex

---

**Q7.** A fleet of 2 million IoT sensors writes temperature readings to Cloud Bigtable every 5 seconds. The application must fetch the most recent readings for a specific device very quickly, and writes must be spread evenly across nodes to avoid hotspotting. Which row key design should be used?

A) Row key = timestamp (epoch milliseconds) only
B) Row key = a randomly generated UUID for each reading
C) Row key = timestamp + device_id
D) Row key = device_id + reverse timestamp (e.g., Long.MAX_VALUE - timestamp)

---

**Q8.** A company has an existing on-premises Hadoop cluster running hundreds of Apache Spark and Hive jobs. They want to migrate to GCP with minimal code rewrite, spin up ephemeral clusters per job to control cost, and retain full control over cluster software versions and configuration. Which service should they use?

A) Cloud Dataproc
B) Cloud Dataflow
C) Cloud Data Fusion
D) Dataform

---

**Q9.** A BigQuery table holds 50 TB of e-commerce order data. Every analytical query filters on a date range using the event_date column, and most queries also filter or join on customer_id, which has millions of distinct values. How should this table be designed to minimize bytes scanned and cost?

A) Partition the table by event_date and cluster by customer_id
B) Partition the table by customer_id and cluster by event_date
C) Do not partition the table; cluster by both customer_id and event_date
D) Partition the table by ingestion time only, and cluster by customer_id

---

**Q10.** A global financial services firm needs a database for a new ledger application that requires strongly consistent, multi-row ACID transactions across users located in North America, Europe, and Asia, horizontal write scalability beyond what a single-region instance can provide, and a SQL interface. Which service should be selected?

A) Cloud SQL with cross-region read replicas
B) Cloud Spanner configured with a multi-region instance
C) Firestore in multi-region mode
D) BigQuery

---

**Q11.** A retail company streams point-of-sale transactions through Pub/Sub into a Dataflow pipeline using fixed 5-minute event-time windows. Store network outages cause some transaction events to arrive up to 10 minutes after their event timestamp. Which configuration ensures the late transactions are still incorporated into results without indefinitely delaying window emission?

A) Increase the window size to 1 hour so all late data falls within the window
B) Set allowed lateness to 10 minutes and use accumulating mode with early and late firing triggers
C) Switch from event-time windows to processing-time windows
D) Rely on the default watermark behavior, which automatically retains all late data forever

---

**Q12.** A company runs an on-premises Oracle database and needs near-real-time change data capture (CDC) replication of table changes into BigQuery for analytics, with minimal operational overhead and no custom code to manage.

A) Data Fusion
B) Dataproc with a custom Sqoop job scheduled hourly
C) Datastream
D) Storage Transfer Service

---

**Q13.** A team is migrating nightly batch Spark jobs from on-premises Hadoop to Google Cloud. Each job runs for about 45 minutes once per day. The team wants to minimize cost and avoid any cluster management overhead.

A) A persistent Dataproc cluster with an autoscaling policy set to a minimum of 2 workers
B) Dataproc on GKE with a fixed node pool sized for peak load
C) Dataproc Serverless for Spark, submitting the job as a batch workload
D) One long-running shared Dataproc cluster kept alive for all nightly jobs across teams

---

**Q14.** A Dataflow streaming pipeline reads JSON events from a Pub/Sub subscription and applies a ParDo transform. Occasionally malformed JSON records cause the transform to throw exceptions, and these exceptions are causing repeated retries that stall the pipeline. What is the recommended fix?

A) Increase the number of Dataflow worker instances to process records faster
B) Increase the Pub/Sub subscription's acknowledgement deadline to its maximum value
C) Switch the pipeline from exactly-once to at-least-once streaming mode
D) Wrap the parsing logic in a try/catch inside the DoFn and route failed elements to a dead-letter output (e.g., a separate Pub/Sub topic) using a TupleTag side output

---

**Q15.** An analytics team wants to build and maintain SQL-based ELT transformations directly against BigQuery tables, with version control, dependency graph management, and built-in data quality testing — without provisioning or managing any Spark or Hadoop clusters.

A) Data Fusion
B) Dataform
C) Dataproc
D) Cloud Composer

---

**Q16.** Which statement correctly describes exactly-once processing semantics in a Dataflow streaming pipeline that reads messages from a Pub/Sub subscription?

A) Pub/Sub itself guarantees exactly-once delivery to every subscriber by default, so no additional handling is needed
B) Achieving exactly-once processing always requires the pipeline author to manually implement deduplication logic using session windows
C) Dataflow's runner tracks Pub/Sub message IDs and deduplicates redelivered messages, providing exactly-once processing semantics within the pipeline even though Pub/Sub delivery itself is at-least-once
D) Exactly-once processing is only available in Dataflow batch pipelines, not in streaming pipelines

---

**Q17.** A retail company runs a production MySQL database (500 GB, high transaction volume) and needs near-real-time replication of table changes into BigQuery for analytics, without adding significant load to the source database or writing custom polling logic. Which approach BEST meets these requirements?

A) Use Datastream to capture change data from the MySQL binary logs and stream changes into BigQuery
B) Build a Dataflow pipeline that polls the MySQL tables via JDBC every minute and writes changed rows to BigQuery
C) Use Dataproc with Sqoop to extract full table snapshots into Cloud Storage nightly, then load into BigQuery
D) Use Cloud Data Fusion to run a nightly batch pipeline that reads the full MySQL tables and overwrites the BigQuery tables

---

**Q18.** A streaming pipeline in Dataflow uses 5-minute fixed windows to aggregate IoT sensor readings. Due to intermittent network issues, some events arrive up to 10 minutes after the window's watermark passes. The team wants late events incorporated into results without letting window state grow unbounded. Which windowing configuration should they use?

A) Increase the fixed window size to 15 minutes so late events always fall within the window
B) Use a single global window with no triggers so all events are eventually included
C) Configure the 5-minute fixed windows with allowedLateness(10 minutes) and an accumulating trigger that fires on the watermark and again for late data, discarding state after the allowed lateness expires
D) Switch to session windows with a 10-minute gap duration to capture late-arriving events

---

**Q19.** A company is migrating on-premises Hadoop/Spark ETL jobs to GCP. The jobs use existing PySpark scripts referencing HDFS paths, and the team wants to minimize code rewrites while reducing idle compute costs between job runs. Which architecture BEST fits these requirements?

A) Use Cloud Composer to orchestrate ephemeral Dataproc clusters with autoscaling policies, using the Cloud Storage connector in place of HDFS, spinning clusters up per job and tearing them down after completion
B) Run the jobs on a single persistent Dataproc cluster with a fixed node count that stays running 24/7
C) Rewrite all the PySpark scripts as Apache Beam pipelines and run them on Dataflow
D) Load the raw data into BigQuery and rewrite all transformation logic as BigQuery SQL scheduled queries

---

**Q20.** A Cloud Composer DAG must wait for a specific file to be uploaded to a Cloud Storage bucket before triggering a downstream Dataflow job. The team wants to avoid wasting Airflow worker slots while waiting, and needs configurable retries and a timeout. Which Airflow component should be used?

A) A PythonOperator that runs a custom while-loop polling the bucket every few seconds
B) A GCSObjectExistenceSensor configured in deferrable mode with a timeout and retry/poke interval
C) A BashOperator that repeatedly calls 'gsutil ls' in a shell loop
D) A TriggerDagRunOperator that starts a second DAG once the file appears

---

**Q21.** A Dataflow streaming pipeline reads from a Pub/Sub subscription and occasionally receives malformed JSON payloads that cause a ParDo transform to throw exceptions, stalling the pipeline. The team needs the pipeline to keep processing valid messages and preserve the malformed ones for later inspection. What is the BEST way to handle this?

A) Wrap the parsing logic in a try/catch inside the DoFn and route unparseable messages to a dead-letter output (via a TupleTag or additional output), publishing them to a separate Pub/Sub dead-letter topic while the main output continues processing valid messages
B) Increase the Pub/Sub subscription's acknowledgement deadline to give the transform more time to process each message
C) Set the Pub/Sub subscription's maximum delivery attempts to unlimited so malformed messages retry indefinitely
D) Disable exactly-once processing on the pipeline to speed up message throughput

---

**Q22.** A company ingests IoT sensor readings into Bigtable at 50,000 writes/second across 10,000 devices. The current row key format is <timestamp>#<device_id>. Cloud Monitoring shows write throughput is uneven, with some nodes heavily overloaded while others are nearly idle. Which row key redesign BEST resolves this hotspotting?

A) Redesign the key as <device_id>#<timestamp>
B) Keep the current format <timestamp>#<device_id> but store timestamps in reverse order
C) Redesign the key as <hash(device_id)>#<timestamp> to force even byte distribution
D) Use a single static prefix such as 'sensor-data' for all row keys

---

**Q23.** A 5-year transaction history table in BigQuery is queried mostly by a transaction_date range, then filtered further by customer_id within that range; occasionally queries filter by customer_id alone across all dates. Which table layout minimizes bytes scanned and cost for these access patterns?

A) Partition by customer_id and cluster by transaction_date
B) Do not partition; cluster by both customer_id and transaction_date
C) Partition by transaction_date (DAY) and cluster by customer_id
D) Partition by transaction_date (YEAR) and add no clustering

---

**Q24.** A media company stores 500TB of raw video footage in Cloud Storage. Files are accessed frequently in the first 30 days, occasionally between day 30 and day 90, and almost never after day 90, but must be retained for 7 years for compliance. Which strategy BEST minimizes cost while meeting the retention requirement?

A) Store everything in Standard storage class for the full 7 years
B) Store in Nearline immediately, then transition to Coldline at day 90
C) Use a single lifecycle rule to move all objects to Archive storage class immediately upon upload
D) Store in Standard for 30 days, transition to Nearline at day 30, transition to Archive at day 90, delete after 7 years

---

**Q25.** A global financial application requires strongly consistent, horizontally scalable relational storage with 99.999% availability across multiple regions and must scale beyond 100TB with automatic sharding and no application-managed partitioning. Which service should you choose?

A) Cloud SQL with cross-region read replicas
B) BigQuery with streaming inserts for transactional writes
C) Cloud Spanner with a multi-region configuration
D) Cloud Bigtable configured with strong consistency for financial records

---

**Q26.** A dataset admin accidentally deletes a BigQuery table 10 days ago and now needs to restore it. The dataset's time travel window is set to the default value. What is the outcome?

A) The table can still be restored directly using the FOR SYSTEM_TIME AS OF clause in the BigQuery console
B) The table is permanently and unconditionally unrecoverable after 7 days under any circumstances
C) The table can be restored using gcloud with no time limit as long as the dataset itself still exists
D) The time travel window (default 7 days) has expired, but the table may still be recoverable during the fail-safe period by contacting Google Cloud Customer Care

---

**Q27.** A company runs BigQuery queries scanning a consistent 200TB per day, 24/7, with a highly predictable workload. Currently billed under on-demand pricing (per TB scanned). Which approach BEST minimizes and stabilizes monthly BigQuery cost for this workload?

A) Purchase BigQuery Editions capacity commitments (slots) sized to the predictable workload
B) Continue using on-demand pricing since it automatically scales with demand
C) Enable BigQuery BI Engine reservations to reduce per-query scan charges
D) Export the data to Cloud Storage and query it as BigQuery external tables to avoid scan charges

---

**Q28.** A company ingests IoT sensor readings into Bigtable at extremely high volume. Row keys are formatted as timestamp-sensorID (e.g., '2026-01-15T10:00:00-sensor123'). The team observes severe write hotspotting concentrated on a small subset of tablet servers, and adding more nodes to the cluster does not resolve the issue. Which row key redesign BEST fixes this?

A) Increase the number of Bigtable nodes in the cluster to spread the load
B) Promote sensorID to the front of the key as sensorID-timestamp (optionally hashed/salted) so writes distribute across the keyspace
C) Append a random numeric suffix after the sensorID portion of the existing key
D) Reverse the timestamp value (e.g., max_timestamp - timestamp) but keep it as the key prefix

---

**Q29.** You are configuring a Cloud Storage lifecycle rule that must transition objects to Nearline storage 30 days after object creation, but the rule should apply ONLY to objects that currently have the STANDARD storage class. Which lifecycle condition configuration correctly achieves this?

A) A single condition using "createdBefore" combined with "numNewerVersions": 1
B) A rule with action Delete and condition "daysSinceCustomTime": 30
C) A rule with action SetStorageClass: NEARLINE and conditions "age": 30 combined with "matchesStorageClass": ["STANDARD"]
D) A rule with action SetStorageClass: NEARLINE and condition "isLive": true only

---

**Q30.** A financial services company is building a new trading ledger application. It requires strongly consistent, multi-row ACID transactions, SQL query support, and the ability to scale writes horizontally across multiple regions as transaction volume grows over time. Which storage service BEST meets these requirements?

A) Cloud Spanner configured with a multi-region instance
B) Cloud SQL with cross-region read replicas
C) Cloud Bigtable with app-level consistency checks
D) BigQuery with a multi-region dataset location

---

**Q31.** A retail company's sales data already lives in BigQuery and is updated nightly via scheduled queries. The data science team wants to build a simple linear regression model to forecast next month's revenue per region. They have SQL skills but no experience with Python or ML frameworks, and want to avoid moving data out of BigQuery. Which approach BEST meets these requirements?

A) Export the BigQuery data to a Vertex AI managed dataset and train a custom TensorFlow model on Vertex AI Training
B) Move the data to Cloud Storage and use Vertex AI AutoML Tables to train the forecasting model
C) Spin up a Dataproc cluster and use Spark MLlib to train a linear regression model
D) Use BigQuery ML to create a linear regression model directly with CREATE MODEL syntax

---

**Q32.** A financial services company serves multiple fraud-detection models in production. They need a centralized way to store, reuse, and serve pre-computed features with low-latency online lookups at prediction time, while also enabling batch feature retrieval for training and monitoring features for drift across teams. Which Google Cloud capability should they use?

A) A Dataflow streaming pipeline writing directly to Memorystore for Redis
B) Scheduled BigQuery queries materializing feature tables nightly
C) Vertex AI Feature Store
D) Cloud Bigtable with custom application code for feature lookups

---

**Q33.** A data platform team wants to securely share curated, frequently updated BigQuery datasets with several external partner organizations. They need to avoid duplicating the underlying data, maintain centralized access control at the source, and allow partners to subscribe to and query the data as if it were their own BigQuery dataset. Which Google Cloud service should they use?

A) BigQuery Data Transfer Service
B) Cloud Pub/Sub with a push subscription per partner
C) Signed URLs on a Cloud Storage export bucket
D) Analytics Hub

---

**Q34.** A team builds a BigQuery ML model using CREATE MODEL, applying several preprocessing functions such as ML.STANDARDIZE and ML.BUCKETIZE to raw input columns before feeding them into the model. They want these exact preprocessing steps to be automatically applied whenever ML.PREDICT is called on new data, without having to manually reapply the transformations at prediction time. Which CREATE MODEL clause should they use?

A) The OPTIONS clause
B) The TRANSFORM clause
C) A nested CREATE MODEL ... AS SELECT subquery
D) The ML.FEATURE_INFO function

---

**Q35.** A dashboard built in Looker Studio queries a BigQuery table that is updated every few minutes with streaming inserts. Business users complain the dashboard shows stale numbers that lag behind the underlying table. The BigQuery connector was configured to use a cached data extract refreshed once per day. What should the team do to ensure the dashboard reflects near real-time data?

A) Switch the BigQuery data source connection from extract mode to a live/direct query connection
B) Increase the extract refresh schedule to run once per week instead of once per day
C) Export the BigQuery table to Google Sheets and connect Looker Studio to the Sheet instead
D) Add a community connector on top of the existing extract to fetch new rows

---

**Q36.** A team retrains an ML model weekly using a BigQuery table that is continuously updated by streaming ingestion. For audit and reproducibility, they need to be able to reference the EXACT state of the training data used for each historical model version, even months later, without relying on BigQuery's default 7-day time travel window. What should they do before each training run?

A) Rely on BigQuery time travel, since it can always recover any past table state on demand
B) Re-run the same training query each time and assume the underlying data hasn't changed
C) Create a BigQuery table snapshot of the source table immediately before each training run and reference that snapshot for reproducibility
D) Manually export the table to a CSV file in Cloud Storage before every training run

---

**Q37.** A retail company stores transaction data in BigQuery and wants to serve pre-computed customer features to both a BigQuery ML model and an online Vertex AI model with low-latency lookups at serving time. The features must stay consistent between training and serving. Which approach BEST meets these requirements?

A) Export features from BigQuery to a Cloud Storage bucket in CSV format and have both training and serving pipelines read directly from the bucket
B) Create a materialized view in BigQuery and query it directly from the online serving application at prediction time
C) Write a custom Cloud Run service that queries BigQuery on every prediction request to compute features on the fly
D) Use Vertex AI Feature Store to ingest features from BigQuery, then use it for both offline training data retrieval and low-latency online serving

---

**Q38.** A company runs a daily Cloud Composer DAG that must complete BigQuery loads by 6 AM for downstream reporting. Recently several tasks silently failed after exhausting retries, and the data team only learned about the missed SLA the next day when analysts complained. Which change BEST ensures the team is notified within minutes of a task failure?

A) Configure Airflow on_failure_callback / email_on_failure to publish to Cloud Monitoring or Pub/Sub, and create a Cloud Monitoring alerting policy on the Composer environment
B) Increase the DAG's retries and retry_delay values so failures resolve automatically without operator intervention
C) Export Cloud Logging DAG logs into BigQuery for post-hoc analysis of failures
D) Change the DAG's schedule_interval to run more frequently so a single missed run has less impact

---

**Q39.** A financial services company ingests raw customer support transcripts into Cloud Storage before loading them into BigQuery. Compliance requires that any Social Security numbers or credit card numbers embedded in the free text be identified and redacted prior to loading. Which Google Cloud service should be used to accomplish this requirement?

A) BigQuery column-level security with policy tags
B) VPC Service Controls perimeter around the Cloud Storage bucket
C) Cloud KMS customer-managed encryption keys on the Cloud Storage bucket
D) Cloud Data Loss Prevention (Sensitive Data Protection) to inspect and de-identify the text before load

---

**Q40.** A data engineer needs to restrict access to the ssn column in a BigQuery table so that only members of finance-pii@company.com can view its values, while other authorized users can query the rest of the table normally. Which approach requires the LEAST ongoing maintenance and is native to BigQuery?

A) Create a separate authorized view per user group exposing only permitted columns
B) Assign a policy tag from a Data Catalog taxonomy to the ssn column and grant the Fine-Grained Reader role on that policy tag to finance-pii@company.com
C) Export the table nightly with the sensitive column removed for general use
D) Apply a row-level access policy that filters rows where ssn is not null

---

**Q41.** A healthcare company stores highly regulated patient data in BigQuery datasets within a dedicated project. Policy requires that this data can never be copied to a BigQuery dataset or Cloud Storage bucket outside the organization's approved projects — even by a user with valid IAM permissions who obtains an OAuth token and attempts the copy from an unauthorized project. Which control should be implemented?

A) Enable CMEK on all BigQuery tables in the project
B) Grant only the BigQuery Data Viewer IAM role to all users
C) Enable Cloud Audit Logs (Data Access logs) on the BigQuery datasets
D) Configure a VPC Service Controls perimeter around the project containing the regulated data, restricting API access and data movement to approved projects

---

**Q42.** Which statement correctly differentiates Customer-Managed Encryption Keys (CMEK) from Google's default encryption in BigQuery?

A) CMEK is required to enable encryption at rest; data is otherwise stored unencrypted
B) With CMEK the customer manages the encryption key in Cloud KMS, including rotation and revocation — disabling or destroying the key renders the data inaccessible; default encryption uses Google-managed keys with no customer-controlled revocation
C) CMEK encrypts data only in transit, while default encryption applies only at rest
D) CMEK is available only for Cloud Storage and cannot be applied to BigQuery datasets

---

**Q43.** A SaaS company stores all customer records in a single shared BigQuery table sales_data. Each customer's analysts query the table through the same reporting dashboard but must only see rows belonging to their own customer_id. Creating a separate table per customer is not feasible given the number of customers. Which BigQuery feature should be used?

A) Create a separate authorized view exposing only the customer_id column for filtering
B) Create row-level access policies (CREATE ROW ACCESS POLICY) on sales_data that filter rows based on the querying user's identity or session variable matching customer_id
C) Apply column-level security policy tags to the customer_id column
D) Use dataset-level IAM conditions to restrict access to the entire dataset per customer

---

**Q44.** A healthcare company stores patient records in a single BigQuery table that includes a SSN column. Data analysts must be able to run aggregate queries over the table but must never see raw SSN values. A separate compliance team must occasionally see the raw SSN for audit purposes. The solution must use a single source table, minimize maintenance overhead, and scale to dozens of similar tables across the organization. Which approach BEST meets these requirements?

A) Implement row-level security policies on the table that filter rows based on the querying user's group membership so analysts only see rows they are authorized for
B) Export the table to Cloud Storage, use Cloud DLP to de-identify the SSN column, and load the de-identified copy into a separate dataset for analysts while compliance keeps access to the original table
C) Create a taxonomy and policy tag in Data Catalog, apply the policy tag to the SSN column, grant the analyst group the 'Masked Reader' role with a masking rule on that policy tag, and grant the compliance group the 'Fine-Grained Reader' role on the same policy tag
D) Create two BigQuery views on top of the source table — one that excludes the SSN column for analysts and one that includes all columns for the compliance team — and grant each group access to its respective view

---

**Q45.** A team runs nightly ETL DAGs in Cloud Composer 2. Finance requires that if any critical task exceeds its expected runtime, an on-call engineer is notified via Cloud Monitoring within 15 minutes, without the task being forcibly terminated. Which approach BEST satisfies this requirement?

A) Set an execution_timeout of 15 minutes on each critical task so Airflow automatically fails the task and triggers the default email_on_failure notification
B) Define an `sla` parameter on each critical task representing its expected duration, implement an `sla_miss_callback` to publish a notification, and create a log-based metric with a Cloud Monitoring alerting policy on SLA-miss log entries
C) Enable the Cloud Composer environment monitoring dashboard in Cloud Monitoring and have the on-call engineer check it each morning before business hours
D) Increase the Cloud Composer worker pool size and node count so tasks complete faster and are less likely to exceed the expected runtime window

---

