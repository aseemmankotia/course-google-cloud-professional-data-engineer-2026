## Chapter 10 Cheat Sheet — Operating & Monitoring Production Data Workloads

### Scheduling
| Tool | Use When | Key Mechanism |
|---|---|---|
| Cloud Composer (Airflow) | Multi-step DAGs, cross-service orchestration, SLA tracking | `sla` + `sla_miss_callback` per task |
| BigQuery Scheduled Queries | Simple recurring SQL transforms | Min repeat interval ~15 min via BQ Data Transfer Service |

### Monitoring & Alerting
| Concept | Detail |
|---|---|
| Cloud Monitoring alert policy | Condition + notification channel; incident auto-closes after **7 days** with no data |
| SLO + burn-rate alert | Detects error-budget consumption trending toward breach — use for gradual freshness/latency degradation |
| Cloud Logging | Log-based metrics feed alert policies; use for pipeline error counts, retry counts |
| Composer SLA | Task/DAG-level `sla=timedelta(...)`, fires `sla_miss_callback` — the exam's answer for 'pipeline SLA alerting' |

### Troubleshooting Dataflow
| Symptom | Cause | Fix |
|---|---|---|
| One worker pegged, others idle, rising lag | Hot key in GroupByKey/Combine | Salt key + two-stage combine, or `Combine.perKey` with fanout |
| Individual slow work items, UI shows 'Stragglers' | Skewed data or slow external I/O per element | Reshuffle, fix external call latency, use `Reshuffle.viaRandomKey()` |
| Job won't drain, watermark stuck | Late data / stuck window / blocked external sink | Check watermark lag metrics, allowed lateness config |
| Drain vs Cancel | Need graceful stop vs immediate stop | `drain` finishes buffered work; `cancel` stops immediately (may drop data) |

### Troubleshooting Dataproc
| Symptom | Cause | Fix |
|---|---|---|
| One task/partition takes far longer than others | Data/key skew in shuffle | Salt key, raise `spark.sql.shuffle.partitions`, repartition |
| Job stalls on preemptible worker loss | No checkpointing, preemptible eviction | Use mix of standard + preemptible secondary workers, enable checkpointing |
| Too many small tasks | Small files problem | Compact input files before job runs |

### Disaster Recovery / Backup
| Resource | Pattern | Key Number |
|---|---|---|
| BigQuery table | Time travel (accidental delete/update recovery) | 2–7 days, default 7 |
| BigQuery table (long-term) | Table snapshot (`bq mk --snapshot`) | Retention independent of time travel |
| BigQuery cross-region DR | Dataset location immutable → scheduled copy/export-load to DR-region dataset | No native replication |
| Cloud Storage cross-region DR | Dual-region bucket + Turbo Replication | RPO ≈ 15 min |
| Cloud SQL | Automated backups + binary log PITR | Retention configurable up to 365 days |

### Data Validation
- Validate schema + row counts pre/post transform (e.g., Dataflow data validation patterns, Great Expectations-style checks).
- Fail pipeline early on schema drift rather than letting bad data reach BigQuery.

**Exam traps:** confusing drain/cancel; assuming autoscaling fixes hot keys/skew; assuming BigQuery datasets replicate across regions automatically; forgetting time travel max is 7 days (snapshots for longer retention).
