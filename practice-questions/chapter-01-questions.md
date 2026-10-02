## Chapter 1: Choosing the Right Storage and Processing Architecture — Practice Questions

**Q1.** A global bank needs a database for its core ledger system. Requirements: strict ACID transactions across regions, horizontal scalability beyond a single machine, SQL interface, and 99.999% availability SLA. Which service BEST meets these requirements?

A) Cloud SQL with read replicas
B) Cloud Bigtable
C) Cloud Spanner (multi-region configuration)
D) Firestore in Native mode

<details>
<summary>Answer</summary>

**Correct: C**

Spanner is the only GCP service offering horizontally scalable relational storage with external consistency and a 99.999% SLA on multi-region configurations.

**Why the others are wrong:**
- Cloud SQL scales vertically only (max ~64TB single instance) and cannot span regions with strong consistency.
- Bigtable has no SQL/ACID multi-row transaction support.
- Firestore supports transactions but is a document NoSQL store, not relational, and lacks the throughput profile for a ledger at global bank scale.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An IoT platform ingests 5 million sensor writes per second and requires single-digit millisecond read/write latency at petabyte scale. Queries are simple key-based lookups with no joins. Which database should be used?

A) Cloud Spanner
B) Cloud Bigtable
C) BigQuery
D) Cloud SQL for PostgreSQL

<details>
<summary>Answer</summary>

**Correct: B**

Bigtable is purpose-built for high-throughput, low-latency, wide-column NoSQL workloads at petabyte scale with simple key-based access patterns.

**Why the others are wrong:**
- Spanner supports high throughput but at higher latency and cost for simple key-value access; it's optimized for relational transactions, not this pattern.
- BigQuery is designed for analytical SQL scans, not single-digit ms operational reads/writes.
- Cloud SQL cannot scale to millions of writes per second.

</details>

---

**Q3.** A retailer wants a clickstream dashboard that updates within seconds of user activity, aggregating events into 1-minute windows before loading into BigQuery. Which architecture is BEST?

A) Cloud Storage batch load into BigQuery every hour via Cloud Composer
B) Direct writes from the app servers into BigQuery using batch load jobs
C) Pub/Sub ingestion with a Dataflow streaming pipeline using fixed windows, writing to BigQuery via the Storage Write API
D) Cloud SQL as the ingestion buffer, replicated nightly into BigQuery

<details>
<summary>Answer</summary>

**Correct: C**

Sub-minute latency requires a streaming pipeline: Pub/Sub for ingestion, Dataflow with fixed windowing for aggregation, and the Storage Write API for low-latency BigQuery writes.

**Why the others are wrong:**
- Hourly batch loads violate the seconds-level latency requirement.
- Batch load jobs are not designed for continuous, low-latency ingestion and have daily load job quotas.
- Cloud SQL with nightly replication introduces hours of latency, failing the requirement.

</details>

---

**Q4.** A mobile app needs a database with automatic offline data sync, real-time listeners for UI updates, and document-based flexible schema for user profiles. Which service fits BEST?

A) Cloud Bigtable
B) BigQuery
C) Firestore
D) Cloud Spanner

<details>
<summary>Answer</summary>

**Correct: C**

Firestore natively supports offline persistence, real-time listeners, and a flexible document model, all built specifically for mobile/web client SDKs.

**Why the others are wrong:**
- Bigtable has no client SDK for offline sync or real-time listeners.
- BigQuery is analytical, not a low-latency transactional client-facing database.
- Spanner supports strong consistency but has no built-in offline sync or real-time listener support for mobile clients.

</details>

---

**Q5.** A data team needs to run ad-hoc SQL analytics over 50 PB of historical sales data with unpredictable query patterns and wants a fully managed, serverless data warehouse. Which service should they choose?

A) Cloud SQL for PostgreSQL
B) Cloud Bigtable
C) BigQuery
D) Cloud Spanner

<details>
<summary>Answer</summary>

**Correct: C**

BigQuery is GCP's serverless, petabyte-scale analytical data warehouse designed exactly for ad-hoc SQL over massive historical datasets.

**Why the others are wrong:**
- Cloud SQL cannot scale beyond tens of TB and isn't optimized for OLAP scans.
- Bigtable has no native SQL analytics engine (requires external query layer).
- Spanner is optimized for OLTP, not large-scale ad-hoc analytical scans.

</details>

---

**Q6.** A BigQuery table stores 3 years of order data. Queries almost always filter on order_date over a date range AND filter/join on customer_id, which has millions of distinct values. What is the BEST table design?

A) Partition by customer_id, cluster by order_date
B) Partition by order_date, cluster by customer_id
C) No partitioning, cluster by both columns
D) Partition by ingestion time only, no clustering

<details>
<summary>Answer</summary>

**Correct: B**

Date-range partitioning prunes scanned partitions for the date filter, and clustering on high-cardinality customer_id further prunes blocks within each partition, minimizing bytes scanned.

**Why the others are wrong:**
- Partitioning requires low-cardinality, range-friendly columns; customer_id with millions of values would exceed practical partition limits and isn't range-filtered.
- Skipping partitioning loses date-range pruning, the most selective and common filter.
- Ingestion-time partitioning without clustering ignores the customer_id filter pattern entirely, forcing full scans within partitions.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A partitioned BigQuery table (partitioned by ingestion date) is frequently filtered by user_id within the WHERE clause, causing high query costs from full-partition scans. What change reduces bytes scanned with the LEAST schema disruption?

A) Re-partition the table by user_id
B) Add clustering on user_id to the existing partitioned table
C) Denormalize user_id into a separate flat table with no partitioning
D) Switch to Bigtable for this workload

<details>
<summary>Answer</summary>

**Correct: B**

Clustering can be added to an existing partitioned table (up to 4 clustering columns) and will prune blocks based on user_id without changing the partitioning strategy.

**Why the others are wrong:**
- user_id is high-cardinality and unsuited for partitioning (partition limit is 10,000 partitions per table).
- Removing partitioning loses date-based pruning entirely and doesn't solve the cost issue.
- Migrating to Bigtable is a major architecture change and unnecessary for a query-cost optimization within BigQuery.

</details>

---

**Q8.** Which statement about denormalization in BigQuery is TRUE?

A) Denormalized schemas using nested and repeated fields always reduce total storage cost compared to normalized tables
B) Denormalization avoids expensive joins at query time but typically increases storage footprint and data duplication
C) BigQuery performs best with fully normalized, third-normal-form schemas like a traditional OLTP database
D) Nested and repeated fields are only supported when using the legacy SQL dialect

<details>
<summary>Answer</summary>

**Correct: B**

Denormalization (nested/repeated STRUCT/ARRAY fields) trades increased storage/duplication for the elimination of costly joins, which fits BigQuery's columnar, distributed execution model.

**Why the others are wrong:**
- Denormalization generally increases storage, not reduces it, due to duplicated data.
- BigQuery's distributed engine performs worse with heavily normalized, join-heavy schemas versus denormalized ones.
- Nested/repeated fields are fully supported in standard SQL and are the recommended modern approach.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

