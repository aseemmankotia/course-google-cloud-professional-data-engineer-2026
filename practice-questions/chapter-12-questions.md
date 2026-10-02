## Chapter 12: Full Exam Simulation and Test-Taking Strategy — Practice Questions

**Q1.** A retail company streams clickstream events through Pub/Sub and needs a 5-minute rolling revenue dashboard with windowed aggregation, exactly-once semantics, and minimal operational overhead. Which solution BEST meets these requirements?

A) Dataproc running Spark Structured Streaming with custom checkpoint management
B) Cloud Functions triggered per message performing manual in-memory aggregation
C) BigQuery scheduled queries every 5 minutes reading from a Pub/Sub export table
D) Dataflow using Apache Beam fixed windows with autoscaling enabled

<details>
<summary>Answer</summary>

**Correct: D**

Dataflow is fully managed, autoscales, natively supports Beam windowing, and provides exactly-once processing guarantees with minimal ops burden.

**Why the others are wrong:**
- Dataproc requires cluster sizing, patching, and manual checkpoint logic — high operational overhead.
- Cloud Functions has no built-in windowing/state management; manual aggregation risks data loss and duplication.
- Scheduled queries introduce polling latency and don't provide true streaming windowed aggregation.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A company needs a wide-column database for time-series sensor data with sub-10ms read latency at roughly 1 million writes per second. Which service should be selected?

A) Firestore
B) Cloud SQL
C) Bigtable
D) BigQuery

<details>
<summary>Answer</summary>

**Correct: C**

Bigtable is purpose-built for high-throughput, low-latency wide-column workloads like time-series and IoT data at massive write scale.

**Why the others are wrong:**
- Firestore is a document store optimized for moderate-throughput transactional app data, not million-writes-per-second workloads.
- Cloud SQL is a relational OLTP engine that cannot scale to this write volume.
- BigQuery is an analytical warehouse with second-to-minute latency, not sub-10ms point lookups.

</details>

---

**Q3.** A BigQuery table is frequently filtered on event_date (spanning 3 years) and high-cardinality customer_id. Which partitioning and clustering configuration is BEST?

A) Partition by event_date, cluster by customer_id
B) Partition by customer_id, cluster by event_date
C) Partition by ingestion time only, no clustering
D) Cluster by both event_date and customer_id, no partitioning

<details>
<summary>Answer</summary>

**Correct: A**

Partitioning on the date column enables partition pruning within the 4,000-partition limit, while clustering on high-cardinality customer_id further sorts data for efficient filtering inside each partition.

**Why the others are wrong:**
- Partitioning requires a low-cardinality date/int column — customer_id would exceed partition limits and provide poor pruning.
- Ingestion-time-only partitioning ignores the actual filter column and wastes clustering benefits.
- Clustering alone without partitioning loses the coarse date-based pruning that reduces bytes scanned.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A Dataflow pipeline must group user session events where a gap of more than 30 minutes between events ends a session, and session lengths vary per user. Which windowing strategy should be used?

A) Fixed windows of 30 minutes
B) Sliding windows of 30 minutes every 5 minutes
C) Session windows with a 30-minute gap duration
D) Global window with periodic 30-minute triggers

<details>
<summary>Answer</summary>

**Correct: C**

Session windows group events based on a configurable gap duration, dynamically sizing each window per user activity — exactly matching the described requirement.

**Why the others are wrong:**
- Fixed windows use rigid, non-overlapping time boundaries regardless of activity gaps, splitting sessions incorrectly.
- Sliding windows overlap on a fixed schedule and don't align with variable gap-based session boundaries.
- A global window with periodic triggers doesn't account for per-user gap-based session termination.

</details>

---

**Q5.** You are 40 questions into a 50-question, 120-minute exam with 25 minutes remaining, and 6 questions are flagged for review. What is the BEST test-taking strategy?

A) Immediately return to the flagged questions before answering the remaining 10 unanswered questions
B) Skip the flagged questions permanently to save time
C) Change answers on flagged questions based on gut feeling without re-reading them
D) Finish all remaining unanswered questions first, then use leftover time to revisit flagged questions

<details>
<summary>Answer</summary>

**Correct: D**

Ensuring every question has an answer guarantees no easy points are lost; only after all questions are attempted should remaining time go to review, since there's no penalty for guessing.

**Why the others are wrong:**
- Returning early to flagged questions risks running out of time before answering the remaining unattempted questions, which is worse than a guessed flagged answer.
- Skipping flagged questions permanently abandons potential points since a best-guess answer is already recorded and could be improved.
- Changing answers without re-reading context can introduce errors; revisions should be based on renewed analysis, not impulse.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A mobile application requires strong consistency, multi-document transactions, and offline data synchronization. Which service is the BEST fit?

A) Bigtable
B) Firestore
C) BigQuery
D) Cloud Spanner

<details>
<summary>Answer</summary>

**Correct: B**

Firestore natively supports offline sync for mobile/web clients along with strong consistency and multi-document ACID transactions, matching all stated requirements.

**Why the others are wrong:**
- Bigtable has no multi-row ACID transaction support and no built-in offline client sync.
- BigQuery is an analytical warehouse, not designed for transactional mobile app data or offline sync.
- Cloud Spanner offers strong consistency and transactions but lacks native mobile offline sync SDKs, making it a poorer fit than Firestore for this specific requirement.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A data analyst needs to run BigQuery queries and view results but must NOT be able to create, delete, or modify datasets or table data. Which IAM configuration satisfies least privilege?

A) roles/bigquery.admin
B) roles/bigquery.user plus roles/bigquery.dataViewer
C) roles/bigquery.dataEditor
D) roles/bigquery.jobUser only

<details>
<summary>Answer</summary>

**Correct: B**

bigquery.user allows running jobs/queries and bigquery.dataViewer grants read-only access to data, together enabling query execution and result viewing without modification rights.

**Why the others are wrong:**
- bigquery.admin grants full control including dataset deletion and modification, violating least privilege.
- dataEditor allows modifying table data and schemas, which exceeds the stated requirement.
- jobUser alone permits running jobs but does not grant read access to the underlying data, so query results referencing tables would fail without dataViewer.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** On a scenario question, two of the four options both describe technically valid GCP services for the use case, but only one satisfies a stated constraint of 'lowest operational overhead.' What elimination technique should you apply?

A) Choose whichever of the two valid options is listed first
B) Re-read the scenario for constraint keywords like serverless, managed, or minimal ops, and eliminate the option requiring cluster management
C) Select both options since either could be considered correct
D) Discard the question and guess randomly to save time

<details>
<summary>Answer</summary>

**Correct: B**

The exam always encodes a decisive constraint in the wording; scanning for operational-overhead keywords (serverless/managed vs. self-managed clusters) resolves ties between otherwise valid services.

**Why the others are wrong:**
- Option order is arbitrary and carries no signal about correctness.
- The exam is single-answer only; selecting both is not a valid response format and guarantees the question is marked wrong.
- Guessing randomly abandons available information; the constraint keyword almost always determines the single correct answer.

</details>

---

