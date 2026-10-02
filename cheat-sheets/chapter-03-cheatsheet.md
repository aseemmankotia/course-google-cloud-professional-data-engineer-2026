## Chapter 3 Cheat Sheet — Pub/Sub & Dataflow Streaming

### Pub/Sub Core Limits
| Item | Value |
|---|---|
| Max message size | 10 MB |
| Default retention | 7 days |
| Retention range | 10 min – 31 days |
| Default ack deadline | 10 sec |
| Max ack deadline | 600 sec (10 min) |
| Dead-letter maxDeliveryAttempts | 5 – 100 |
| Ordering key throughput | ~1 MB/s per key per region |

### Subscription Types
| Type | Use When |
|---|---|
| Push | Reachable HTTPS endpoint, low latency, server-to-server |
| Pull | Client-controlled fetch, unreliable connectivity, high throughput |

### Pub/Sub vs Pub/Sub Lite
| | Pub/Sub | Pub/Sub Lite |
|---|---|---|
| Scaling | Fully managed autoscale | Manual partitions/zonal capacity |
| Cost | Higher per GB | Lower at high sustained throughput |
| Availability | Regional/global | Zonal |

### Beam Windowing
| Window | Syntax | Use Case |
|---|---|---|
| Fixed | `FixedWindows.of(Duration)` | Per-interval aggregates (e.g., per-minute counts) |
| Sliding | `SlidingWindows.of(size).every(period)` | Moving averages/trends |
| Session | `Sessions.withGapDuration(Duration)` | Per-user activity bursts with inactivity gap |

### Watermarks & Late Data
- Watermark = heuristic estimate that all event-time data up to X has arrived.
- Default trigger: `AfterWatermark.pastEndOfWindow()` — fires once, drops late data.
- Use `.withAllowedLateness(Duration)` + late trigger (`.withLateFirings(...)`) to keep window state open and emit updated results for late arrivals.
- Exam trap: forgetting allowed lateness silently **drops** late data instead of erroring.

### Exactly-once vs At-least-once
| | Guarantee | Where |
|---|---|---|
| At-least-once | No loss, possible duplicates | Pub/Sub default delivery |
| Exactly-once processing | No loss, no duplicates within pipeline | Dataflow runner default (record ID dedup) |
| Exactly-once delivery | No duplicate delivery to subscriber | Optional Pub/Sub subscription feature |
- Exam trap: assuming Pub/Sub delivery itself is exactly-once by default — it's at-least-once unless the explicit feature is enabled.

### Dead-Letter Handling
- Configure subscription-level dead-letter topic + maxDeliveryAttempts (5–100).
- In Dataflow code: catch parse/processing exceptions, use `TupleTag` side outputs to route bad records to a DLQ sink (topic/table), keep main path flowing.

### Autoscaling / Throughput Tuning
- Enable **Streaming Engine** for efficient state handling + smaller workers.
- Horizontal (throughput-based) autoscaling adjusts worker count based on backlog/CPU.
- Tune via `--maxNumWorkers`; avoid fixed `--numWorkers` for variable traffic.
- Exam trap: lowering ack deadline to 'speed up' processing — actually increases duplicate redelivery under backlog.
