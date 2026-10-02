## Chapter 6: BigQuery Storage Optimization — Practice Questions

**Q1.** A company has a 500TB BigQuery table of clickstream events. Analysts almost always filter by event_date and then by user_country, which has 220 distinct values. Which design minimizes bytes scanned and cost?

A) Partition by user_country and cluster by event_date
B) Partition by event_date and cluster by user_country
C) Cluster by both event_date and user_country, no partitioning
D) Partition by ingestion time only, no clustering

<details>
<summary>Answer</summary>

**Correct: B**

Time-unit partitioning on event_date lets BigQuery prune whole partitions for date-range filters, and clustering on user_country (a lower-cardinality, frequently-filtered column) further sorts data within each partition for block pruning.

**Why the others are wrong:**
- Partitioning requires a DATE/TIMESTAMP/INTEGER column with bounded cardinality — partitioning by a 220-value string column would create excessive/awkward partitions and isn't how BigQuery time-unit partitioning works for this use case
- Without partitioning, BigQuery cannot prune at the partition (metadata) level — clustering alone only reorders blocks, it does not eliminate reading based on date the way partition pruning does at scale
- Ingestion time only ignores the actual event_date, so filters on event_date won't align with partition boundaries, causing full scans when late-arriving data exists

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A startup runs unpredictable, bursty analytics — sometimes zero queries for days, then a spike of large ad-hoc queries. Which pricing model BEST fits and why?

A) Flat-rate/Enterprise Edition reservation sized for peak load
B) Enterprise Plus Edition with autoscaling reservations only
C) On-demand pricing, since cost is billed per query per bytes scanned with no idle reservation cost
D) Physical bytes storage billing model to reduce compute cost

<details>
<summary>Answer</summary>

**Correct: C**

On-demand pricing (currently $6.25 per TiB scanned, with 1 TiB/month free) charges only for actual query bytes scanned — ideal for unpredictable, low-average-utilization workloads with no ongoing slot commitment.

**Why the others are wrong:**
- A flat-rate/edition reservation sized for peak load pays for idle capacity during the many low-usage days, wasting money
- Enterprise Plus autoscaling still requires baseline slot commitments and per-slot-hour billing that's less economical than on-demand for rare bursty usage
- Physical bytes storage billing is a storage pricing option, unrelated to compute/query cost — it doesn't address the query cost question at all

</details>

---

**Q3.** A finance dashboard needs near-real-time aggregates (within a few minutes) of a constantly-updated 2TB transactions table, with automatic incremental updates and BigQuery automatically rewriting queries to use the pre-aggregated data when beneficial. Which feature should be used?

A) A scheduled query running every 5 minutes that overwrites a destination table
B) A daily export to Cloud Storage with a scheduled reload
C) A view (non-materialized) with the same aggregation logic
D) A materialized view over the transactions table

<details>
<summary>Answer</summary>

**Correct: D**

Materialized views incrementally and automatically refresh as base table data changes, and BigQuery's query optimizer can automatically rewrite eligible queries against the base table to use the materialized view for lower latency and cost — no orchestration needed.

**Why the others are wrong:**
- Scheduled queries fully re-run and overwrite on a fixed cron schedule; they don't incrementally refresh, don't offer automatic query rewrite, and 5-minute full recomputation over a 2TB table is costly and not truly near-real-time
- Exporting to Cloud Storage adds latency and removes the automatic query rewrite/optimization benefit entirely
- A standard (non-materialized) view re-executes the full underlying query on every read, giving no precomputation benefit and no cost/latency improvement

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A table must be partitioned on a numeric customer_id column ranging from 1 to 10,000,000, where analysts frequently filter on customer_id ranges (e.g., 1–100000). Which partitioning strategy applies?

A) Ingestion-time partitioning
B) Integer-range partitioning on customer_id
C) Time-unit partitioning on a DATE column
D) Clustering only, since partitioning requires a date/timestamp column

<details>
<summary>Answer</summary>

**Correct: B**

Integer-range partitioning lets you define a start, end, and interval on an INTEGER column like customer_id, enabling partition pruning for numeric range filters.

**Why the others are wrong:**
- Ingestion-time partitioning uses load/arrival time, unrelated to customer_id and wouldn't support customer_id range filters
- There's no DATE column involved in this scenario, so time-unit partitioning doesn't apply
- Partitioning does not require a date/timestamp column — BigQuery explicitly supports INTEGER-range partitioning; this option's premise is factually wrong

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** Which statement correctly distinguishes partitioning from clustering in BigQuery?

A) Partitioning divides a table into segments with metadata that BigQuery can prune before scanning; clustering sorts data within storage blocks to improve pruning for high-cardinality filter/sort columns
B) Partitioning and clustering are interchangeable; both divide the table into physically separate partitions with metadata pruning
C) Clustering is limited to 1 column while partitioning supports unlimited columns
D) Clustering requires a DATE or TIMESTAMP column, while partitioning can use any data type

<details>
<summary>Answer</summary>

**Correct: A**

Partitioning creates distinct, addressable segments (max 4,000 per table) prunable via metadata; clustering sorts rows within storage blocks by up to 4 columns, useful for higher-cardinality columns where full partitioning isn't practical.

**Why the others are wrong:**
- Clustering does not create separate physical partitions with independent metadata pruning the way partitioning does — they operate at different storage granularities
- It's reversed: clustering supports up to 4 columns; a table supports exactly one partitioning column/expression
- Clustering has no data-type restriction to DATE/TIMESTAMP; partitioning is the one restricted to DATE, TIMESTAMP, DATETIME, or INTEGER-range columns

</details>

---

**Q6.** What is the maximum number of clustering columns allowed on a single BigQuery table?

A) 1
B) 4
C) 10
D) Unlimited

<details>
<summary>Answer</summary>

**Correct: B**

BigQuery allows up to 4 clustering columns per table, and column order matters — put the most frequently filtered/highest-benefit column first.

**Why the others are wrong:**
- 1 confuses clustering with the single-column partitioning limit
- 10 overstates the actual limit, a commonly guessed but incorrect number
- Clustering is not unlimited; exceeding 4 columns is rejected at table creation/DDL time

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** By default, how many days of time travel does BigQuery provide for a dataset, and what is the minimum configurable value?

A) Default 30 days, configurable down to 7 days
B) Default 7 days, non-configurable
C) Default 1 day, configurable up to 7 days
D) Default 7 days, configurable down to 2 days

<details>
<summary>Answer</summary>

**Correct: D**

BigQuery's default time travel window is 7 days at the dataset level, and it can be configured down to as little as 2 days (or up to 7) to reduce storage costs from retained changed/deleted data.

**Why the others are wrong:**
- 30 days default overstates BigQuery's window; that duration is not the default nor the max in current BigQuery time travel
- It IS configurable via the dataset's max_time_travel_hours setting, so calling it non-configurable is incorrect
- 1 day is not the default — 7 days is, and 7 is the maximum, not a value you configure 'up to' from a 1-day default

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement best explains why BigQuery's storage/compute separation matters for cost optimization decisions on the exam?

A) Storage and compute must always scale together, so increasing slots automatically increases storage cost
B) Because storage and compute are combined into one bill, there is no way to reduce cost independently
C) Compute is billed only when data is stored for more than 90 days
D) Storage is billed independently of query compute, so you can optimize storage tier (active vs long-term) and compute pricing model (on-demand vs editions/slots) separately based on access patterns

<details>
<summary>Answer</summary>

**Correct: D**

BigQuery decouples storage billing (active vs. long-term storage rates, e.g. tables untouched 90+ days automatically drop to lower long-term storage pricing) from compute billing (on-demand per-bytes-scanned or edition/slot-based), letting engineers independently tune each dimension for cost.

**Why the others are wrong:**
- Storage and compute do NOT scale together — that's the entire point of BigQuery's serverless, decoupled architecture
- They are billed separately, not combined, which is exactly why independent optimization is possible
- This conflates the long-term storage discount trigger (90 days no modification) with compute billing, which are unrelated mechanisms

</details>

---

