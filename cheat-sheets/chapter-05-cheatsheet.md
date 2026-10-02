## Chapter 5 Cheat Sheet — Data Fusion, Dataform, Composer

### Tool Selection Matrix
| Requirement | Best Tool | Why |
|---|---|---|
| No-code, multi-source ETL (on-prem, JDBC, files) | **Data Fusion** | Visual studio, 150+ connectors, runs on Dataproc |
| SQL-only transforms already in BigQuery, version control, tests | **Dataform** | SQLX + ref() dependency graph, no infra |
| Orchestrate multiple tools/dependencies/retries/SLAs | **Cloud Composer** | Managed Airflow, cross-service operators |
| Custom code, streaming, complex windowing | **Dataflow** | Beam-based, autoscaling workers |

### Data Fusion Editions
| Edition | HA | Versioning | Real-time | Use Case |
|---|---|---|---|---|
| Basic | No | No | No | Simple batch, low cost |
| Enterprise | Yes | Yes | Yes | Production |
| Developer | No | Limited | No | Dev/test only, single node |

### Dataform Essentials
- Files: `.sqlx` — contain `config {...}` block + SQL body
- `config.type`: `table` | `view` | `incremental` | `declaration`
- `${ref('name')}` — creates query AND dependency edge
- Assertions: `config { assertions: { uniqueKey: [...] } }`
- No compute cost beyond BigQuery query/slot usage

### Airflow / Composer Key Params
| Param | Purpose | Default |
|---|---|---|
| `retries` | # retry attempts | 0 |
| `retry_delay` | wait between retries | none (must set) |
| `retry_exponential_backoff` | exponential growth of delay | False |
| `sla` | alert if not done by X after DAG scheduled start | None |
| `execution_timeout` | kills task if runtime exceeds value | None |
| `trigger_rule` | condition for downstream task to run | `all_success` |

### Common trigger_rule values
`all_success` (default) · `all_failed` · `one_failed` · `all_done` · `none_failed` — use for dead-letter/notify branches.

### Composer 2 Environment Sizing
Small / Medium / Large — controls scheduler, worker, web server, triggerer resource allocation; worker autoscaling min/max set independently.

### Dead-Letter Patterns
- **Pub/Sub**: dead-letter topic, min **5** / max **100** delivery attempts before forwarding.
- **Dataflow**: TupleTag split → main output + dead-letter PCollection → separate BQ table/topic.
- **Composer**: failure branch task via `trigger_rule='one_failed'` → notify/log.

### Exam Traps
- SLA miss ≠ task failure — task can still succeed after SLA alert fires.
- Data Fusion is NOT serverless by default — pipelines provision Dataproc clusters (cost implication).
- Dataform has no separate billing — cost hides inside BigQuery job costs.
- Forgetting `retries` in default_args means **zero** automatic retries, even with retry_delay set.
