## Storage/Processing Decision Cheat Sheet

### Database Selection Matrix
| Requirement | Service |
|---|---|
| OLTP app backend, <64TB, single region | Cloud SQL |
| Global relational, ACID across rows, horizontal scale | Cloud Spanner |
| NoSQL wide-column, huge throughput, single-digit ms, key-based access | Bigtable |
| Mobile/web app, offline sync, real-time listeners, document model | Firestore |
| Analytical SQL, ad-hoc queries, TB–PB scale, serverless | BigQuery |

### Key Numbers
| Item | Value |
|---|---|
| BigQuery max partitions/table | 10,000 |
| BigQuery max clustering columns | 4 |
| Cloud SQL max storage/instance | 64TB |
| Spanner SLA (regional / multi-region) | 99.99% / 99.999% |
| Pub/Sub retention (default / max) | 7 days / 31 days |
| Firestore max document size | 1MB |

### Batch vs Streaming
- **Batch**: Cloud Storage → Dataflow/Dataproc batch job → BigQuery load. Use when latency of hours is acceptable; cheaper.
- **Streaming**: Pub/Sub → Dataflow (streaming, windowed) → BigQuery Storage Write API. Use when latency of seconds is required.
- **Lambda architecture**: batch (accuracy) + speed layer (low latency), merged at serving layer — complex, two codebases.
- **Kappa architecture**: single streaming pipeline, replay Pub/Sub log for reprocessing — simpler, GCP-preferred modern pattern.

### BigQuery Partitioning & Clustering Strategy
1. Partition by the column with the most common **range/date filter** (low-to-medium cardinality).
2. Cluster (up to 4 cols) by **high-cardinality equality filter/join** columns, most selective first.
3. Partition pruning happens before clustering pruning — always design partitioning first.
4. Avoid partitioning on high-cardinality columns (e.g., user_id) — use clustering instead.

### Denormalization vs Normalization
- **Denormalize** (nested/repeated STRUCT/ARRAY) in BigQuery: avoids joins, but increases storage & duplication. Preferred pattern for BigQuery.
- **Normalize** in Cloud SQL/Spanner: reduces duplication, supports multi-table ACID transactions, standard OLTP design.

### Reliability/Scalability/Cost/Latency Trade-offs
- More 9's of availability (Spanner multi-region) = higher cost & write latency.
- BigQuery on-demand pricing = pay-per-scan; Editions (Standard/Enterprise/Enterprise Plus) = flat-rate slot pricing for predictable, high-volume workloads.
- Bigtable/Spanner scale horizontally by adding nodes; Cloud SQL scales vertically (single machine ceiling).

### Exam Traps
- Choosing Bigtable for workloads needing SQL joins or ACID — wrong, use Spanner.
- Choosing BigQuery for low-latency transactional app backend — wrong, it's OLAP not OLTP.
- Partitioning by a high-cardinality column instead of clustering — exceeds partition limits.
- Assuming denormalization reduces storage — it increases storage but reduces query-time joins.
