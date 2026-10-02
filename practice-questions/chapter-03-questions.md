## Chapter 3: Streaming Ingestion with Pub/Sub and Dataflow — Practice Questions

**Q1.** A retail company streams clickstream events through Pub/Sub into Dataflow. Analysts want to know, for each user, how long a continuous browsing 'session' lasted, where a session ends after 10 minutes of inactivity. Which windowing strategy should the pipeline use?

A) Fixed windows of 10 minutes
B) Sliding windows of 10 minutes every 1 minute
C) Global window with a repeating AfterCount trigger
D) Session windows with a 10-minute gap duration

<details>
<summary>Answer</summary>

**Correct: D**

Session windows group events per key based on periods of activity separated by a gap of inactivity — exactly the 'ends after 10 minutes idle' requirement. Each user gets dynamically sized windows.

**Why the others are wrong:**
- Fixed windows produce uniform time buckets regardless of activity gaps, splitting a continuous session across multiple windows.
- Sliding windows overlap and are for smoothing/moving aggregates, not for grouping by inactivity gaps.
- Global window with triggers can approximate this but is unnecessarily complex and not the standard exam answer for session-based grouping.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A pipeline must guarantee that a mobile client receives Pub/Sub messages the instant they are published, without the client polling. Network connectivity to the client is unreliable and intermittent. Which subscription type should you choose?

A) Push subscription delivering directly to the mobile client
B) Push subscription to an HTTPS endpoint behind a load balancer, which then notifies the client
C) BigQuery subscription writing directly to a table the client queries
D) Pull subscription with the client polling every second

<details>
<summary>Answer</summary>

**Correct: D**

Pull subscriptions are recommended when the subscriber has unreliable or intermittent connectivity, or high throughput needs, because the client controls when and how much to fetch and can retry independently.

**Why the others are wrong:**
- Push subscriptions require a stable, reachable HTTPS endpoint owned by the client; a mobile device with intermittent connectivity cannot reliably act as that endpoint.
- Push to a load balancer still requires a reachable endpoint and doesn't solve the mobile client's intermittent connectivity for final delivery.
- BigQuery subscriptions are for analytics ingestion, not real-time client delivery.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A Dataflow streaming pipeline occasionally receives malformed JSON records from Pub/Sub that fail parsing and crash the DoFn, stalling the pipeline. What is the BEST way to handle this without losing throughput?

A) Set the subscription's dead-letter policy to forward all messages after 1 delivery attempt
B) Wrap the parsing logic in a try/catch, route failures to a dead-letter output (e.g., a side output written to a DLQ topic/table), and continue processing valid records
C) Increase the ack deadline to 600 seconds so the DoFn has more time to parse
D) Delete the malformed messages manually from the Pub/Sub topic before Dataflow reads them

<details>
<summary>Answer</summary>

**Correct: B**

The standard Beam/Dataflow pattern for streaming error handling is to catch parse exceptions, tag bad records with TupleTags, and write them to a dead-letter sink for later inspection, while good records flow through unaffected.

**Why the others are wrong:**
- A delivery attempt threshold of 1 sends nearly every message (even transient failures) to dead-letter immediately, which is too aggressive and doesn't fix pipeline-level parsing crashes.
- Ack deadline controls Pub/Sub redelivery timing, not whether malformed JSON parses successfully — it won't fix a crash.
- Manually deleting messages doesn't scale and doesn't address the root parsing issue in the pipeline code.

</details>

---

**Q4.** You need extremely low-cost, high-throughput event ingestion (dozens of MB/s per partition) for a well-known, fixed number of consumers, and you are willing to manage partition/zone capacity yourself. Which service should you choose over standard Pub/Sub?

A) Cloud Tasks
B) Pub/Sub Lite
C) Standard Pub/Sub with more subscriptions
D) Cloud Storage with object notifications

<details>
<summary>Answer</summary>

**Correct: B**

Pub/Sub Lite offers zonal, partition-based storage with lower cost at high, predictable throughput, but requires you to provision and manage capacity per partition/zone — the exact tradeoff described.

**Why the others are wrong:**
- Cloud Tasks is for task queues/deferred execution, not high-throughput streaming ingestion.
- Standard Pub/Sub auto-scales and is fully managed, but costs more per GB at very high sustained throughput and doesn't require partition management.
- Cloud Storage notifications are for object lifecycle events, not general-purpose streaming ingestion.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** In a Dataflow streaming pipeline, the watermark for a window has passed, but 2% of events for that window arrive 3 minutes later due to a mobile network delay. The business requirement is to still include these late events in the aggregate if they arrive within 5 minutes of the watermark passing. Which configuration should you use?

A) Set withAllowedLateness(Duration.standardMinutes(5)) and a trigger with late firings enabled
B) Set the trigger to AfterWatermark only, with no allowed lateness
C) Increase the window size to 5 minutes larger than needed
D) Set the Pub/Sub ack deadline to 5 minutes

<details>
<summary>Answer</summary>

**Correct: A**

withAllowedLateness extends how long a window's state is kept after the watermark passes so late data can still trigger updated (late) pane firings — this directly matches a 5-minute late-arrival tolerance requirement.

**Why the others are wrong:**
- AfterWatermark with no allowed lateness discards data arriving after the watermark, dropping the late 2% entirely.
- Enlarging the window changes aggregation semantics and doesn't target late-arrival tolerance specifically — it also delays all results by that same window size.
- The Pub/Sub ack deadline governs message redelivery timing to Dataflow, not how long Beam keeps window state open for late data.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** You configure a Pub/Sub subscription's dead-letter policy. Which value range is valid for the maximum delivery attempts before a message is forwarded to the dead-letter topic?

A) 1 to 10
B) 10 to 1000
C) Unlimited — set to 0 for no limit
D) 5 to 100

<details>
<summary>Answer</summary>

**Correct: D**

Pub/Sub's dead-letter policy requires maxDeliveryAttempts to be between 5 and 100 (inclusive); the exam tests this exact numeric range.

**Why the others are wrong:**
- 1 to 10 is too narrow and below the allowed minimum floor in places — the true minimum is 5.
- 10 to 1000 exceeds the actual maximum of 100.
- There is no unlimited/0 option — a numeric value within 5–100 is mandatory.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A Dataflow streaming job processes financial transactions and must never double-count a transaction in its aggregated output, even if Pub/Sub redelivers a message due to a network blip. Which statement about exactly-once semantics is correct?

A) Pub/Sub guarantees exactly-once delivery to every subscriber by default with no configuration needed
B) Dataflow's runner provides exactly-once processing semantics for records within the pipeline by default, deduplicating redelivered Pub/Sub messages using record IDs
C) Exactly-once semantics only exist in batch pipelines, never in streaming
D) You must manually deduplicate every message using Cloud SQL to achieve exactly-once results

<details>
<summary>Answer</summary>

**Correct: B**

Dataflow provides exactly-once processing of records through the pipeline by default (using per-record IDs and checkpointing), even though the underlying Pub/Sub delivery to Dataflow is at-least-once — Dataflow handles the deduplication.

**Why the others are wrong:**
- Pub/Sub's baseline delivery guarantee is at-least-once; exactly-once delivery is an optional, explicitly enabled subscription feature, not the unconditional default.
- Exactly-once processing semantics are a core Dataflow/Beam streaming feature, not exclusive to batch.
- Manual deduplication via an external database is unnecessary overhead — Dataflow's runner already handles this internally.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A streaming Dataflow pipeline reading from Pub/Sub is falling behind, with a growing subscription backlog during traffic spikes, then catching up during quiet periods. What is the BEST first step to address this using Dataflow's built-in capability?

A) Manually add more workers via a fixed --numWorkers value and redeploy for every spike
B) Reduce the Pub/Sub subscription's ack deadline to force faster redelivery
C) Enable Streaming Engine and Dataflow's horizontal autoscaling so worker count adjusts automatically based on backlog
D) Switch the pipeline from streaming mode to batch mode to process backlog in bulk

<details>
<summary>Answer</summary>

**Correct: C**

Dataflow's Streaming Engine combined with horizontal (throughput-based) autoscaling automatically scales worker count up during backlog spikes and down during quiet periods, which is the built-in, exam-recommended mechanism for exactly this scenario.

**Why the others are wrong:**
- Manually redeploying with a fixed worker count for every spike is operationally unscalable and defeats the purpose of a managed autoscaling service.
- Lowering the ack deadline increases redelivery of unacked messages, worsening backlog and duplicate processing rather than fixing throughput.
- Streaming pipelines are architected for continuous processing; switching to batch changes the processing model entirely and isn't a 'first step' tuning action.

</details>

---

