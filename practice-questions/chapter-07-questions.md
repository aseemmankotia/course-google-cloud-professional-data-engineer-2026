## Chapter 7: Bigtable, Spanner, Cloud SQL, and Cloud Storage — Practice Questions

**Q1.** A company ingests IoT sensor readings at 500,000 writes per second across 2 million devices. They plan to use Bigtable with a row key of deviceID#timestamp. During load testing, a small number of nodes receive most of the write traffic while others sit idle. What is the BEST fix?

A) Increase the number of Bigtable nodes to spread the load automatically
B) Reverse or hash the deviceID as a prefix so writes are distributed randomly across the row key space, keeping timestamp as a suffix
C) Switch the row key to timestamp#deviceID so writes are ordered chronologically
D) Change the storage type from SSD to HDD to increase write throughput per node

<details>
<summary>Answer</summary>

**Correct: B**

Prefixing with a hashed/salted deviceID (or field promotion) spreads writes across the keyspace and thus across tablets/nodes, eliminating hotspotting while timestamp suffix still allows range scans per device.

**Why the others are wrong:**
- Adding nodes doesn't fix hotspotting — the issue is key distribution, not raw capacity
- Leading with timestamp is worse — all writes at the same moment go to the same tablet, causing severe hotspotting
- HDD is slower than SSD for most workloads and does not address key distribution at all

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A global financial services firm needs a relational database with synchronous multi-region replication, external strong consistency for transactions, and the ability to scale to petabytes with SQL support. Which service BEST meets these requirements?

A) Cloud SQL with cross-region read replicas
B) Bigtable with multi-cluster routing
C) Cloud Spanner with a multi-region configuration
D) BigQuery with partitioned tables

<details>
<summary>Answer</summary>

**Correct: C**

Spanner is the only option offering synchronous multi-region replication with external strong consistency (via TrueTime) and horizontal SQL scalability to petabytes, backed by a 99.999% multi-region SLA.

**Why the others are wrong:**
- Cloud SQL read replicas are asynchronous and eventually consistent, and Cloud SQL cannot scale horizontally to petabytes
- Bigtable has no SQL support and no transactional/strong consistency guarantees across rows
- BigQuery is an analytical warehouse, not a transactional (OLTP) system

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A media company stores 800TB of video archive footage that is accessed roughly twice a year for compliance audits. Files must never be deleted for 7 years due to legal hold. Which storage class configuration is MOST cost-effective?

A) Standard storage class with no lifecycle rule
B) Nearline storage class with a 30-day lifecycle transition
C) Archive storage class, since access is less than once per year
D) Coldline storage class, since access is quarterly

<details>
<summary>Answer</summary>

**Correct: C**

Archive is designed for data accessed less than once a year, has the lowest storage cost of all classes, and matches the 7-year retention need despite its 365-day minimum storage duration and higher retrieval cost, which is acceptable given rare access.

**Why the others are wrong:**
- Standard is by far the most expensive option for rarely accessed data
- Nearline targets data accessed about once a month, not twice a year — it's more expensive than needed
- Coldline targets roughly quarterly access; twice-yearly access fits Archive's less-than-once-a-year profile better and is cheaper

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** An application requires a fully managed MySQL database for an internal HR system with fewer than 50 concurrent users, standard ACID transactions, and no need for global distribution. Which service should be selected?

A) Cloud Spanner
B) Bigtable
C) Firestore
D) Cloud SQL

<details>
<summary>Answer</summary>

**Correct: D**

Cloud SQL provides fully managed MySQL/PostgreSQL/SQL Server with ACID transactions and is the right-sized, lower-cost choice for regional workloads without global scale requirements.

**Why the others are wrong:**
- Spanner is designed for global scale and horizontal growth — overkill and costlier for a 50-user regional workload
- Bigtable is a NoSQL wide-column store with no SQL or multi-row transaction support
- Firestore is a NoSQL document database, not a relational MySQL-compatible engine

</details>

---

**Q5.** Which Bigtable design practice is a well-known anti-pattern that the exam frequently tests as a source of hotspotting?

A) Using tall and narrow tables with many rows and few columns
B) Grouping related columns into column families
C) Designing row keys to support efficient range scans for related entities
D) Using a monotonically increasing value, such as an auto-incrementing ID or unmodified current timestamp, as the leading part of the row key

<details>
<summary>Answer</summary>

**Correct: D**

Monotonically increasing keys concentrate all new writes onto the single tablet holding the highest key range, creating a hotspot; this is the classic exam trap for Bigtable schema design.

**Why the others are wrong:**
- Tall/narrow tables are a recommended Bigtable design pattern, not an anti-pattern
- Column families are a normal, encouraged part of schema design for grouping access patterns
- Range-scan-friendly key design is a best practice when done correctly (e.g., reverse domain names), not an anti-pattern

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A team configures a Cloud Storage bucket lifecycle rule intending to move objects to Nearline after 30 days and delete them after 365 days. They set the rule with an 'Age' condition of 30 for the SetStorageClass action to Nearline, but forget to add a separate rule for deletion. What will happen?

A) Objects will be automatically deleted after 365 days by default even without a rule
B) The lifecycle engine will throw an error at bucket creation time due to the incomplete configuration
C) Objects will revert back to Standard storage class after 365 days automatically
D) Objects will remain in Nearline indefinitely because no Delete action rule exists

<details>
<summary>Answer</summary>

**Correct: D**

Cloud Storage lifecycle rules are independent and explicit — each action (SetStorageClass, Delete) requires its own rule with matching conditions. Without a Delete rule, objects simply stay in Nearline forever.

**Why the others are wrong:**
- There is no default deletion behavior in GCS; objects persist until explicitly deleted
- GCS does not validate business intent — an incomplete lifecycle configuration is syntactically valid and won't error
- Storage classes never auto-revert to a cheaper/more expensive tier without an explicit rule

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which statement correctly compares Bigtable and Cloud Spanner for the exam?

A) Bigtable is a NoSQL wide-column store optimized for high-throughput single-row operations, while Spanner is a relational store offering multi-row/multi-table ACID transactions with SQL
B) Both support full SQL and multi-row ACID transactions across the entire dataset
C) Bigtable offers global strong consistency while Spanner only offers eventual consistency
D) Spanner is schemaless like Bigtable, storing arbitrary column families per row

<details>
<summary>Answer</summary>

**Correct: A**

Bigtable is a schemaless, NoSQL wide-column database best for high-throughput, low-latency single-row read/writes (e.g., time series, IoT); Spanner is a relational database with SQL and horizontally scalable multi-row ACID transactions.

**Why the others are wrong:**
- Bigtable has no SQL interface and only guarantees atomicity at the single-row level, not multi-row
- This reverses reality — Spanner provides external strong consistency globally via TrueTime; Bigtable provides eventual consistency across replicated clusters
- Spanner enforces a defined relational schema with tables and indexes, unlike Bigtable's flexible column families

</details>

---

**Q8.** A retail analytics company needs to store clickstream events with extremely high write throughput (2 million events/sec), needs only key-based lookups by user session ID, and does not require SQL joins. Which service is the BEST fit?

A) Bigtable with a well-distributed row key based on session ID
B) Cloud SQL with sharding across multiple instances
C) Cloud Spanner with interleaved tables
D) Cloud Storage with JSON files partitioned by hour

<details>
<summary>Answer</summary>

**Correct: A**

Bigtable is purpose-built for extremely high-throughput, low-latency key-value/wide-column workloads with simple lookups by key, scaling linearly by adding nodes — ideal for clickstream and time-series data.

**Why the others are wrong:**
- Cloud SQL cannot scale to millions of writes/sec even when manually sharded — it's not designed for this throughput
- Spanner adds SQL/transactional overhead not needed here and is costlier for pure high-throughput key-value access
- Cloud Storage is an object store, not suited for low-latency per-record lookups at this write rate

</details>

---

