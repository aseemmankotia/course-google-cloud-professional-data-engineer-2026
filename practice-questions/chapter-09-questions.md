## Chapter 9: BI, Visualization, and Data Sharing — Practice Questions

**Q1.** A retail company wants a single governed semantic layer so that 'revenue' is calculated identically across 40 dashboards used by finance, marketing, and ops. Business logic must be version-controlled and peer-reviewed before going to production. Which tool BEST fits this requirement?

A) Looker Studio with a shared calculated field copied into each report
B) Looker, defining metrics once in LookML and using Git-based development workflow
C) Connected Sheets with a shared spreadsheet template
D) BigQuery views queried directly by each team without a BI layer

<details>
<summary>Answer</summary>

**Correct: B**

LookML centralizes metric/business-logic definitions in one modeling layer with Git-based version control, code review, and validation before deploying to production, guaranteeing one definition of 'revenue' everywhere.

**Why the others are wrong:**
- Copying a calculated field into every Looker Studio report causes definition drift since each copy can be edited independently with no central governance.
- Connected Sheets has no semantic modeling layer or version control; formulas can diverge per sheet.
- Raw BigQuery views give no centralized business-logic governance and each team could still write inconsistent aggregation SQL.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A data provider publishes a 5TB BigQuery dataset as a listing in an Analytics Hub exchange. A subscriber subscribes and runs analytical queries against the linked dataset for a month. Who is billed for the subscriber's query compute costs?

A) The subscriber's billing account, because Analytics Hub does not move or copy data
B) The data provider's billing account, because they own the underlying data
C) Google Cloud absorbs the cost as part of Analytics Hub service fees
D) Cost is split 50/50 automatically between provider and subscriber projects

<details>
<summary>Answer</summary>

**Correct: A**

Analytics Hub linked datasets are read-only references with no data copy. The provider pays only for storage; each subscriber is billed in their own project for the compute (bytes scanned) of their own queries.

**Why the others are wrong:**
- The provider only pays storage, not subscriber query compute — this is the exact exam trap the domain calls out.
- There is no cost-absorption model in Analytics Hub; standard BigQuery pricing rules apply per project.
- There is no automatic cost-splitting mechanism between provider and subscriber.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A Looker Studio report connects to BigQuery using a single service account configured as 'Owner's Credentials' and is shared with 200 viewers across the company. Finance is concerned about ballooning BigQuery costs. What is the MOST accurate explanation of the risk?

A) Each of the 200 viewers is billed individually for their own BigQuery scans
B) Owner's Credentials mode disables BigQuery billing entirely
C) All 200 viewers' queries are billed to the single owner's project, and every uncached interaction can trigger a scan against that project's quota
D) Looker Studio always uses on-demand pricing regardless of credential mode

<details>
<summary>Answer</summary>

**Correct: C**

With Owner's Credentials, every viewer's interaction runs under the report owner's BigQuery permissions and billing project, so cost and quota exposure concentrate on one project — this is the key cost-governance exam trap for BI connections.

**Why the others are wrong:**
- That behavior describes Viewer's Credentials mode, not Owner's Credentials.
- Owner's Credentials does not disable billing — it just centralizes it under one project.
- Pricing depends on the underlying BigQuery project (on-demand or flat-rate/reservation), not fixed by Looker Studio.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A BI team's Looker Studio dashboards query a 2TB BigQuery table repeatedly throughout the day, and query costs are rising. Which GCP feature should be added to accelerate queries and reduce repeated scan costs without changing the dashboards?

A) Enable BigQuery BI Engine reservation on the dataset's project/location
B) Migrate the data into Cloud Bigtable
C) Increase the BigQuery on-demand slot limit
D) Switch the dashboard to Looker with no other changes

<details>
<summary>Answer</summary>

**Correct: A**

BI Engine provides an in-memory analysis layer that caches and accelerates BigQuery SQL used by BI tools like Looker Studio and Looker, reducing both latency and repeated bytes-scanned cost, with reservations startable from 1 GB.

**Why the others are wrong:**
- Bigtable is a NoSQL wide-column store; migrating would require re-architecting the entire pipeline, not a targeted BI acceleration fix.
- On-demand pricing has no adjustable 'slot limit' in the way flat-rate reservations do; this doesn't address repeated scan cost.
- Switching BI tools alone does not add caching/acceleration — BI Engine (or materialized views) is the actual fix.

</details>

---

**Q5.** A finance analyst wants to build ad-hoc pivot tables and perform live 'what-if' calculations directly on BigQuery data inside a spreadsheet interface, without writing SQL. Which tool is the BEST fit?

A) Connected Sheets
B) Looker Studio
C) Looker with LookML explores
D) Analytics Hub

<details>
<summary>Answer</summary>

**Correct: A**

Connected Sheets lets analysts build pivot tables and formulas against BigQuery data directly in Google Sheets, using BigQuery's compute without SQL — ideal for spreadsheet-native ad-hoc analysis.

**Why the others are wrong:**
- Looker Studio is dashboard/report-focused, not a pivot-table spreadsheet experience.
- Looker requires LookML modeling overhead unsuitable for quick spreadsheet-style ad-hoc work.
- Analytics Hub is a data-sharing exchange mechanism, not an analysis or visualization tool.

</details>

---

**Q6.** A company needs self-service Looker exploration for regional sales managers, but each manager must only see rows for their own region, and the underlying explores must remain centrally governed. Which Looker feature enables this?

A) Separate LookML projects duplicated per region
B) User attributes combined with access filters/row-level security in the LookML model
C) Giving each manager their own BigQuery project
D) Looker Studio filter controls applied per viewer

<details>
<summary>Answer</summary>

**Correct: B**

Looker's user attributes let you inject per-user values (e.g., region) into access filters/row-level security defined once in the shared LookML model, achieving governed self-service without duplicating models.

**Why the others are wrong:**
- Duplicating LookML projects per region breaks the single-source-of-truth governance goal and is unmaintainable.
- Giving separate BigQuery projects doesn't scope row access within a shared explore and adds unnecessary infrastructure.
- Looker Studio filter controls are report-level UI filters set by viewers, not enforced security — anyone can remove them.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** In LookML, a derived table is marked as persistent (a PDT) with a datagroup-based trigger. What is the primary exam-relevant benefit of using a PDT instead of a regular (non-persistent) derived table?

A) PDTs materialize the query result into the database and are only rebuilt when the trigger condition fires, improving performance and controlling cost
B) PDTs are computed fresh on every dashboard load, guaranteeing real-time accuracy
C) PDTs eliminate the need for any BigQuery compute entirely
D) PDTs automatically replicate data into Analytics Hub for sharing

<details>
<summary>Answer</summary>

**Correct: A**

A Persistent Derived Table materializes its result set in the database and is rebuilt only when its datagroup trigger (e.g., a schedule or data change check) fires, avoiding recomputation on every query and reducing cost/latency.

**Why the others are wrong:**
- This describes the opposite of a PDT — non-persistent derived tables are recomputed at query time.
- PDTs still require BigQuery compute to build and periodically refresh the materialized table.
- PDTs are unrelated to Analytics Hub, which is a separate dataset-sharing mechanism.

</details>

---

**Q8.** What is the correct relationship between an Analytics Hub 'exchange' and a 'listing'?

A) An exchange is a container that groups listings and controls who can discover/subscribe to them; a listing is a reference to one shareable dataset
B) A listing contains many exchanges, each representing one shared table
C) An exchange and a listing are interchangeable terms for the same object
D) A listing is created automatically for every BigQuery dataset in a project

<details>
<summary>Answer</summary>

**Correct: A**

An exchange is the governance container (public or private) that groups one or more listings and controls visibility/subscription permissions; each listing references a specific shareable BigQuery dataset.

**Why the others are wrong:**
- This reverses the containment relationship — exchanges contain listings, not the other way around.
- They are distinct objects with different roles (container vs. reference item).
- Listings must be explicitly published by a data provider; they are not auto-created for every dataset.

</details>

---

