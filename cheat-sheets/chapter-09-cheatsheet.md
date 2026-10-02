## Chapter 9 Cheat Sheet — BI, Visualization & Data Sharing

### Looker vs Looker Studio
| Feature | Looker Studio | Looker |
|---|---|---|
| Cost | Free (Pro tier adds SLA/support) | Enterprise platform (licensed) |
| Modeling layer | None — per-report calc fields | LookML semantic model |
| Version control | None native | Git-based, code review |
| Governance/RLS | Limited (viewer filters) | User attributes + access filters |
| Best for | Quick self-service reports | Centralized, reusable enterprise metrics |

**Exam rule:** "one metric definition across many dashboards, governed, auditable" → **Looker**. "quick free dashboard, minimal setup" → **Looker Studio**.

### Analytics Hub
| Object | Purpose |
|---|---|
| Exchange | Container controlling who can discover/subscribe (public/private) |
| Listing | Reference to one shareable BigQuery dataset |
| Linked dataset | Read-only dataset in subscriber project — **no data copy** |

**Billing:** Provider pays storage only. Subscriber pays their own query compute. No data movement/ETL required — near-real-time sharing.

### Looker Studio BigQuery Connection Modes
| Mode | Billing project | Risk |
|---|---|---|
| Owner's Credentials | Owner's project for ALL viewers | Cost concentration/blowup with many viewers |
| Viewer's Credentials | Each viewer's own project | Each viewer needs BQ access/permissions |

### LookML Basics
- `.model.lkml` → connections & explores
- `.view.lkml` → dimensions/measures/tables
- Explore = joined view(s) exposed to users
- **PDT** (Persistent Derived Table): materialized, rebuilt on datagroup trigger → performance/cost win
- Non-persistent derived table: recomputed every query
- Git workflow (dev branch → validate → deploy) = reproducibility & auditability

### Governed Self-Service
- Use **user attributes** + `access_filters` for row-level security in shared explores
- Avoid duplicating LookML per team/region — breaks single-source-of-truth

### Cost-Aware BI Connections to BigQuery
1. **BI Engine**: in-memory acceleration for Looker Studio/Looker queries; reservation 1 GB–100 GB self-service (more via support)
2. **Materialized views**: pre-aggregate repeated dashboard queries, reduce bytes scanned
3. Avoid raw `SELECT *`/custom-query mode without caching — triggers full scans on every refresh
4. Prefer flat-rate/reservation billing for predictable BI workload cost
5. Watch **credential mode** in Looker Studio — Owner's Credentials centralizes cost risk

### Reproducibility & Reliability
- Centralize logic in LookML (Looker) vs scattered calculated fields (Looker Studio) to prevent metric drift
- PDT datagroup triggers + BI Engine caching control both freshness and cost
- Analytics Hub linked datasets stay in sync automatically — no stale copies to reconcile
