## Ch.11 Cheat Sheet — Security & Compliance for Data Pipelines

### IAM: Predefined vs Custom (Least Privilege)
| Role | Grants |
|---|---|
| roles/bigquery.jobUser | Run jobs only, no data access |
| roles/bigquery.dataViewer | Read data + metadata in dataset |
| roles/bigquery.dataEditor | Read/write data |
| roles/bigquery.user | Run jobs + create datasets (project level) |
| roles/bigquery.admin | Full control |

Use **custom roles** when a predefined role grants too much (e.g., needs `bigquery.tables.getData` but not `bigquery.jobs.create`). Exam trap: picking `bigquery.user` when `jobUser` (narrower) is sufficient.

### VPC-SC vs IAM
| | IAM | VPC Service Controls |
|---|---|---|
| Controls | What an identity can do | What can cross a perimeter |
| Protects against | Unauthorized actions | Data exfiltration (even w/ valid creds) |
| Layer | Identity/authorization | Network/context |
| Complement | Works inside perimeter | Wraps around IAM as defense-in-depth |

### Column vs Row-Level Security (BigQuery)
- **Column-level**: Dataplex Catalog policy tag on column → default masked → grant `roles/datacatalog.categoryFineGrainedReader` to unmask.
- **Row-level**: `CREATE ROW ACCESS POLICY name ON ds.table GRANT TO ("group:x") FILTER USING (col = SESSION_USER())`.
- Both can be combined on the same table.

### CMEK & Encryption
- Supported: BigQuery, Cloud Storage, Pub/Sub, Dataflow, Dataproc.
- KMS key rotation: minimum **24 hours**; automatic rotation recommended for compliance.
- CMEK controls **key access**, not query-level authorization — still need IAM/row/column security.

### Cloud DLP (Sensitive Data Protection)
- **Discovery**: inspection jobs use built-in infoTypes (PERSON_NAME, US_SSN, PHONE_NUMBER, etc.), score = **VERY_UNLIKELY → UNLIKELY → POSSIBLE → LIKELY → VERY_LIKELY** (5 levels).
- **De-identification techniques**: masking, tokenization/FPE (reversible), bucketing/generalization, date shifting, redaction.
- Use FPE/tokenization when re-identification is required later; use masking/redaction when it's not.

### Data Residency & Retention
- Org policy `constraints/gcp.resourceLocations` — restrict resource creation to approved regions org-wide.
- BigQuery dataset location is set at creation and **cannot be changed** (must recreate/copy to move regions).
- Time travel: **2–7 days configurable, default 7**. Partition expiration and table expiration control long-term retention.

### Common Exam Traps
1. Choosing VPC-SC when the scenario is really about *what a user can do* (should be IAM/roles).
2. Choosing dataset-level IAM when the requirement is per-column or per-row (should be policy tags / row access policy).
3. Confusing CMEK (key control) with access control (IAM) — CMEK does not restrict who can query decrypted data.
4. Forgetting time travel max is 7 days, not 30.
