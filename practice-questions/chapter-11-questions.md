## Chapter 11: Security and Compliance for Data Pipelines — Practice Questions

**Q1.** A retailer stores customer records in a single BigQuery table. Compliance auditors must be able to see the full SSN column, but 200 other analysts should see it as NULL. All users need the same rows. Which approach BEST meets this requirement with the least ongoing maintenance?

A) Create a separate dataset per team and use IAM dataset-level roles to control access to the whole table
B) Apply a Dataplex Catalog policy tag to the SSN column with a data masking policy, and grant the Fine-Grained Reader role only to the compliance group
C) Export the SSN column to a separate encrypted Cloud Storage bucket and delete it from BigQuery
D) Enable a VPC Service Controls perimeter around the BigQuery project

<details>
<summary>Answer</summary>

**Correct: B**

Column-level security via policy tags plus a masking policy lets one physical table serve both groups: masked by default, unmasked only for principals granted roles/datacatalog.categoryFineGrainedReader on that tag.

**Why the others are wrong:**
- Dataset-per-team duplicates data and still exposes the raw column to whoever has dataset access — it doesn't solve column-level masking
- Removing the column from BigQuery breaks the same-rows requirement and adds an unnecessary separate pipeline
- VPC-SC controls perimeter/API access, not which columns a user can see inside a table they're already authorized to query

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** An attacker obtains a stolen OAuth token with valid roles/bigquery.dataViewer on a project. The security team wants to guarantee that even with valid credentials, the data cannot be copied to a BigQuery dataset in an external, non-org project. Which control enforces this?

A) A VPC Service Controls perimeter around the project
B) IAM Conditions restricting the role to business hours
C) Customer-managed encryption keys (CMEK) on the dataset
D) Cloud DLP inspection job on the dataset

<details>
<summary>Answer</summary>

**Correct: A**

VPC-SC creates a service perimeter that blocks data movement (e.g., BigQuery Data Transfer, copy jobs) to resources outside the perimeter, regardless of whether the caller's IAM credentials are valid — it stops exfiltration that IAM alone cannot.

**Why the others are wrong:**
- IAM Conditions still authorize the same actions within the time window; they don't block exfiltration to another project
- CMEK protects data at rest from unauthorized decryption outside Google's control, but a valid IAM principal with the key can still copy the decrypted data out
- DLP discovers/de-identifies sensitive data; it does not enforce network/perimeter boundaries

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** You need each regional sales rep to query the same shared BigQuery table but only ever see rows for their own region, without maintaining per-region tables. Which feature should you implement?

A) Column-level security with policy tags
B) A row access policy using CREATE ROW ACCESS POLICY with a FILTER USING clause
C) A separate authorized view for each region
D) IAM Conditions on the bigquery.dataViewer role

<details>
<summary>Answer</summary>

**Correct: B**

BigQuery row-level security is implemented with CREATE ROW ACCESS POLICY ... FILTER USING (region_column = SESSION_USER()), enforcing row filtering per user on one physical table with no view maintenance.

**Why the others are wrong:**
- Column-level security hides whole columns, not rows, and doesn't address per-region row filtering
- Authorized views per region work but require creating and maintaining N views — more operational overhead than a single row access policy
- IAM Conditions can restrict access by request attributes like time or resource tags, but cannot filter individual rows of query results

</details>

---

**Q4.** Which statement correctly distinguishes VPC Service Controls from IAM?

A) VPC-SC defines a security perimeter around resources to restrict data exfiltration between services/projects; IAM controls what actions an authenticated identity can perform on a resource
B) IAM defines a network perimeter around resources; VPC-SC defines what actions an identity can perform
C) VPC-SC replaces the need for IAM roles entirely once configured
D) IAM and VPC-SC both operate at the API-call authorization layer and are functionally interchangeable

<details>
<summary>Answer</summary>

**Correct: A**

This is the exact perimeter-vs-identity distinction the exam tests: IAM = 'can this identity do X', VPC-SC = 'can this resource/API call cross this boundary regardless of identity'.

**Why the others are wrong:**
- This reverses the definitions of IAM and VPC-SC
- VPC-SC is a defense-in-depth layer on top of IAM, not a replacement — IAM authorization still applies inside the perimeter
- They operate at different layers (identity/action vs. network/context) and both must be configured — they are complementary, not interchangeable

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A team accidentally dropped a production BigQuery table and needs to restore it using time travel. The dataset was created with default settings. What is the maximum number of days back they can recover the table?

A) 2 days
B) 14 days
C) 7 days
D) 30 days

<details>
<summary>Answer</summary>

**Correct: C**

BigQuery time travel supports a configurable window of 2 to 7 days at the dataset level, and the default (and maximum) is 7 days.

**Why the others are wrong:**
- 2 days is the minimum configurable value, not the default maximum
- 14 days exceeds the maximum supported time travel window
- 30 days is not a supported time travel value for BigQuery

</details>

---

**Q6.** A healthcare company has millions of unstructured PDF documents in Cloud Storage that may contain patient names, medical record numbers, and phone numbers. They need to automatically find and classify this sensitive data before deciding on a retention policy. Which service should they use first?

A) BigQuery row access policies
B) Cloud DLP (Sensitive Data Protection) with an inspection job using built-in infoTypes
C) VPC Service Controls
D) Customer-managed encryption keys (CMEK)

<details>
<summary>Answer</summary>

**Correct: B**

Cloud DLP inspection jobs scan Cloud Storage content using built-in infoTypes (PERSON_NAME, US_HEALTHCARE_NPI, PHONE_NUMBER, etc.) and return likelihood scores, which is the discovery step before any masking/retention decision.

**Why the others are wrong:**
- Row access policies only apply to structured BigQuery tables, not unstructured files in Cloud Storage
- VPC-SC restricts data movement across perimeters; it does not inspect content for sensitive data
- CMEK only manages the encryption key used at rest; it has no data discovery/classification capability

</details>

---

**Q7.** You are configuring a Cloud KMS key that will be used as CMEK for a BigQuery dataset and want automatic key rotation for compliance. What is the minimum rotation period Cloud KMS allows?

A) 24 hours
B) 1 hour
C) 7 days
D) 90 days

<details>
<summary>Answer</summary>

**Correct: A**

Cloud KMS enforces a minimum automatic rotation period of 24 hours (1 day) for symmetric keys used as CMEK.

**Why the others are wrong:**
- 1 hour is shorter than the platform-enforced minimum
- 7 days is a valid rotation value but not the minimum allowed
- 90 days is a common compliance default but far above the actual minimum, so it's not the correct answer to 'minimum period'

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A multinational company must ensure that customer data for its EU subsidiary is never stored or processed outside the EU, enforced at the organization level rather than relying on engineers choosing the right region each time. Which control should they configure?

A) Apply the organization policy constraint constraints/gcp.resourceLocations restricted to EU locations
B) Set the BigQuery dataset default location to 'EU' for each new dataset manually
C) Enable CMEK on all EU datasets
D) Configure a VPC-SC perimeter around the EU project

<details>
<summary>Answer</summary>

**Correct: A**

The gcp.resourceLocations organization policy constraint enforces, at the org/folder/project level, that resources can only be created in specified regions — removing reliance on individual engineers to select the correct region each time.

**Why the others are wrong:**
- Manual per-dataset selection is error-prone and not an org-level enforced control — it relies on human compliance
- CMEK controls who can decrypt data with a key; it has no effect on which geographic region the data is stored in
- VPC-SC restricts API/data-exfiltration paths across perimeters, not the physical region where resources are provisioned

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

