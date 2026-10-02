## Chapter 3: Streaming Ingestion with Pub/Sub and Dataflow — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Pub/Sub maximum message size? | 10 MB per message. Exam tip: larger payloads must be stored elsewhere (e.g., GCS) with a reference sent via Pub/Sub. |
| 2 | Pub/Sub message retention: default and maximum? | Default 7 days; configurable from 10 minutes up to 31 days. Exam tip: extended retention enables replay via seek(). |
| 3 | Pub/Sub ack deadline: default and max? | Default 10 seconds; can be extended up to 600 seconds (10 minutes). Exam tip: too-short deadlines cause duplicate redeliveries under slow processing. |
| 4 | Pub/Sub dead-letter policy: valid maxDeliveryAttempts range? | 5 to 100 (inclusive). Exam tip: below 5 or above 100 is invalid configuration. |
| 5 | Fixed vs Sliding vs Session windows — when to use each? | Fixed: uniform non-overlapping time buckets (e.g., per-minute counts). Sliding: overlapping windows for moving averages/trends. Session: per-key windows closed by an inactivity gap (e.g., user sessions). |
| 6 | What is a watermark in Beam/Dataflow streaming? | A heuristic estimate of event time up to which all data is believed to have arrived. Exam tip: drives when windows are considered 'complete' and default triggers fire. |
| 7 | Push vs Pull Pub/Sub subscriptions — when to choose each? | Push: server-to-server delivery to a reachable HTTPS endpoint, low-latency, no polling. Pull: client controls fetch rate, better for unreliable connectivity or high-throughput batch consumption. |
| 8 | Pub/Sub vs Pub/Sub Lite — key difference? | Pub/Sub: fully managed, auto-scaling, global/regional, higher cost per GB. Pub/Sub Lite: zonal, partition-based, cheaper at high sustained throughput, requires manual capacity/partition management. |
| 9 | Beam syntax: fixed 1-minute window | Window.into(FixedWindows.of(Duration.standardMinutes(1))). Exam tip: pairs with a default AfterWatermark trigger unless overridden. |
| 10 | Beam syntax: allow 5 minutes of lateness with late firing | .withAllowedLateness(Duration.standardMinutes(5)) combined with a trigger using .withLateFirings(AfterCount.of(1)). Exam tip: without allowed lateness, late data is dropped after the watermark passes. |
| 11 | Exactly-once vs at-least-once — what does each guarantee? | At-least-once: message delivered one or more times (possible duplicates), Pub/Sub's baseline guarantee. Exactly-once: no duplicates and no loss — Dataflow provides exactly-once processing internally by default; Pub/Sub also offers an explicit exactly-once delivery feature. |
| 12 | Dataflow Streaming Engine — what does it enable? | Offloads pipeline state/shuffle to the Dataflow service backend, enabling smaller worker VMs and more responsive horizontal (throughput-based) autoscaling for streaming jobs. Exam tip: recommended for backlog-driven scaling scenarios. |
