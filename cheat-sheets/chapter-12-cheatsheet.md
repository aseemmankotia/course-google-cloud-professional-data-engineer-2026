# Chapter 12 Cheat Sheet: Exam Day Strategy

## Time Budget
| Metric | Value |
|---|---|
| Total questions | ~50-60 |
| Total time | 120 min |
| Budget/question | ~2 min |
| Stuck threshold | >3 min → flag & move on |
| Review pass | Reserve last 10-15 min |

## Triage Strategy
1. Read the final sentence (the actual ask) first.
2. Identify domain + constraint keywords (cost, latency, compliance, managed/serverless, real-time).
3. Record a best-guess answer immediately — never leave blank.
4. Flag if confidence <90%; keep moving.

## Elimination Technique
| Signal | Action |
|---|---|
| Absolute words (always/never/only) | Usually wrong |
| Deprecated/legacy service named | Eliminate |
| Requires manual cluster/ops when scenario says 'serverless'/'minimal ops' | Eliminate |
| Fails stated SLA/latency/cost constraint | Eliminate |
| Matches exact keyword + constraint | Likely correct |

## Cross-Domain Trap Patterns
- Storage tier chosen by **access frequency**, not size (Standard/Nearline/Coldline/Archive).
- 'Real-time'/'low latency' → Dataflow/Pub/Sub, not batch or scheduled BigQuery queries.
- IAM: predefined role granularity (jobUser vs dataViewer vs dataEditor) — least-privilege traps.
- Partition = coarse date/int filter for pruning; Cluster = fine-grained sort within partition.
- Bigtable = single-row key design, no multi-row transactions; use Firestore/Spanner for ACID needs.
- Windowing: Fixed (regular), Sliding (overlapping), Session (gap-based) — match to described data pattern.

## Domain-by-Domain Recap
| Domain | Core Decision Point |
|---|---|
| Ingestion/Processing | Batch vs streaming; managed service choice |
| Storage | Structured vs semi-structured vs object; consistency/latency needs |
| Data Modeling | Partition/cluster/schema design |
| Orchestration | Dataflow vs Dataproc vs Composer |
| Security/Governance | IAM roles, encryption, DLP |
| ML/Analytics | BigQuery ML vs Vertex AI vs pre-trained APIs |
| Reliability | SLAs, retries, dead-letter queues, monitoring |

## Final Reminders
- No penalty for guessing — never leave a question blank.
- Flag and continue; don't spiral on one question.
- Re-read constraint keywords before choosing between two 'valid-looking' services.
- Trust your first careful read; second-guessing without new information is often wrong.
