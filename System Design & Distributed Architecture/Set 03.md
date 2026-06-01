# System Design & Distributed Architecture: Set 3
## Async Communication, Message Patterns & Distributed Workflows

## 📘 10 Conceptual Questions
1. **Queues vs. Event Streams:** When should you use a traditional queue (RabbitMQ/Redis) for task execution vs. an event stream (Kafka/Redis Streams) for event sourcing and replayability? How does retention differ, and what happens if a consumer falls behind?
2. **Message Ordering Guarantees:** How do you guarantee processing order when scaling consumers horizontally? Explain partition keys, sharding strategies, and why global ordering often kills throughput.
3. **At-Least-Once vs. Exactly-Once Delivery:** Why is "exactly-once" so hard to achieve? How do you design consumers to be idempotent so that duplicate messages don't corrupt data or double-charge users?
4. **The Outbox Pattern & Transactional Messaging:** How do you guarantee a database update and a message publication happen atomically? Why is publishing from inside a DB transaction risky, and how does the Outbox table solve this?
5. **Backpressure & Flow Control:** What happens when producers outpace consumers? Explain backpressure mechanisms (queue limits, acknowledgment delays, dynamic scaling) and how to prevent memory exhaustion or message loss.
6. **Saga Pattern for Distributed Transactions:** How do you coordinate multi-step workflows across services without a global lock? Compare Choreography (event-driven) vs. Orchestration (central coordinator) and explain compensation logic.
7. **Dead Letter Queues (DLQ) & Poison Messages:** How do you handle messages that fail repeatedly? Explain isolation strategies, manual replay workflows, and how to prevent one bad message from blocking a partition.
8. **Batch Processing vs. Real-Time Streaming:** When should you aggregate data into batches vs. processing event-by-event? How do you balance latency vs. throughput, and what are the checkpointing requirements for stream processing?
9. **Circuit Breakers in Async Systems:** How do you prevent a failing downstream service from backing up your entire message queue? Explain trip thresholds, fallback strategies, and how to avoid "retry storms" when the service recovers.
10. **Observability in Distributed Systems:** How do you trace a request across Web → Queue → Worker → External API? Explain correlation IDs, structured logging, metric aggregation, and how to debug "lost" messages without grep-hunting logs.

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Idempotent Job Implementation**<br>Create a job that charges a user or sends an email. Use a unique idempotency key in the payload. Simulate duplicate deliveries (replay same job). Verify action happens exactly once. | Duplicate jobs return early/silently. Side effect (charge/email) happens once. Idempotency key stored/checked reliably. Logs confirm deduplication. |
| 2 | **Outbox Pattern Implementation**<br>Create `outbox_events` table. Insert event row within same DB transaction as business write. Build publisher job to read unprocessed rows, dispatch to queue/webhook, mark sent. | Events never lost if queue fails. Publisher runs idempotently. Exactly-once delivery verified. Dead-letter queue captures unrecoverable events. |
| 3 | **Saga Workflow with Compensation**<br>Build a 3-step workflow: `ReserveStock → ProcessPayment → NotifyUser`. If Payment fails, trigger `ReleaseStock` compensation job. Track state. Allow manual retry/resume. | All steps execute in order. Failure triggers correct compensation. State persisted between steps. Manual retry endpoint works. No orphaned reservations. |
| 4 | **Dead Letter Queue Handling**<br>Configure queue to move failed jobs to DLQ after 3 retries. Build dashboard/command to view DLQ contents, inspect payload, fix data, and re-enqueue. | Failed jobs isolate after retries. Main queue keeps flowing. DLQ viewer shows payload/error. Re-enqueue works with modified payload. Metrics track DLQ size. |
| 5 | **Backpressure Simulation**<br>Simulate slow consumer (sleep/delay) and fast producer. Monitor queue depth. Implement backpressure: pause producer or scale consumers when depth > threshold. Verify memory stability. | Queue depth stays within bounds. Producer pauses/scales correctly. No OOM or dropped messages. Metrics alert on high depth. |
| 6 | **Ordered Processing by Partition**<br>Dispatch jobs with partition keys (e.g., `user_id`). Configure queue to process same key sequentially. Verify parallel processing works for different keys but order is preserved per key. | Jobs for User A run in order. Jobs for User B run in parallel with A. Order verified via timestamps/logs. No race conditions on user state. |
| 7 | **Batch Import Pipeline**<br>Create command to import 100k records from CSV. Use chunking, batch dispatching, and progress tracking. Implement error handling per row (skip bad rows, continue). Monitor memory/CPU. | Import completes without timeout. Memory stays flat. Bad rows logged/skipped. Progress bar updates. Zero data corruption. |
| 8 | **Fan-Out Notification System**<br>Build event that fans out to multiple channels (Email, SMS, Push). Use separate jobs per channel. Implement graceful degradation: if SMS fails, Email still sends. | Event triggers all jobs. Failures in one channel don't block others. Each job handles errors independently. Metrics show delivery rates per channel. |
| 9 | **Circuit Breaker for Downstream API**<br>Wrap external API call in job with circuit breaker. Track failures, open circuit after threshold, return cached/fallback response. Auto-close after recovery. Log state transitions. | Circuit opens/closes correctly. Fallback data served during outage. No cascading failures. State transitions logged & auditable. |
| 10| **Async Observability Setup**<br>Add `X-Request-ID` propagation to jobs. Log job start/end with duration, payload size, and result. Set up alert on job failure rate > 5%. Verify you can trace job from web trigger to completion. | Trace ID links web request to job execution. Logs contain structured metadata. Alert triggers on anomaly. Debugging workflow documented. |
