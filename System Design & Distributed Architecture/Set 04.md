# System Design & Distributed Architecture: Set 4
## Reliability, Chaos Engineering & Production Resilience

## 📘 10 Conceptual Questions
1. **The Three Pillars of Observability:** How do Metrics, Logs, and Traces complement each other? Why is tracing essential for debugging distributed async workflows (web → queue → database) that logs alone can't solve?
2. **Chaos Engineering & Anti-Fragility:** What is the goal of intentionally injecting failure (killing workers, adding latency)? How do you distinguish between "chaos testing" and "sabotage," and what safety guardrails prevent it from becoming a real incident?
3. **RTO (Recovery Time Objective) vs. RPO (Recovery Point Objective):** How do you define these for different system tiers? Why does a strict RPO often conflict with performance, and how do asynchronous replication strategies balance this trade-off?
4. **Alert Fatigue & Signal-to-Noise:** Why is "alerting on everything" as dangerous as alerting on nothing? How do you design actionable alerts vs. informational logs, and what is the "Golden Signals" methodology (Latency, Traffic, Errors, Saturation)?
5. **Canary Deployments & Blue/Green Strategies:** How do you release new code to 5% of users to catch bugs before full rollout? Compare feature-flag-based canaries vs. infrastructure-based blue/green switches. When does the complexity outweigh the safety?
6. **Circuit Breaker Patterns:** How do you prevent a failing downstream dependency (e.g., slow payment API) from exhausting all your threads/connections? Explain the "Half-Open" state and how it safely tests recovery without a "retry storm."
7. **Idempotency in Distributed Systems:** Why must every public API and background job be idempotent? How do you implement idempotency keys to handle network retries safely without double-processing orders or sending duplicate emails?
8. **Graceful Degradation & Fallbacks:** When a core service fails, how do you keep the app usable? Compare caching fallbacks, disabling non-essential features (e.g., "Reviews are temporarily unavailable"), and queuing requests for later processing.
9. **Rate Limiting Algorithms:** Compare Fixed Window, Sliding Window, and Token Bucket. How does the Token Bucket algorithm allow for "burst" traffic while maintaining a steady average rate, and why is it preferred for API gateways?
10. **Blameless Post-Mortems:** When an incident occurs, why is the focus on "process failure" rather than "human error"? How do you document a timeline, root cause, and action items to prevent recurrence without creating a culture of fear?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Distributed Tracing Implementation** | Inject a unique `Correlation-ID` into every HTTP request. Pass it to queued jobs and log files. Verify you can track a single user action from the Controller → Job → Database using only that ID. |
| 2 | **Chaos Engineering Drill** | Write a script that randomly terminates 50% of your queue workers while the queue is processing. Observe if jobs are lost or if the remaining workers pick them up. Verify system recovers automatically when workers restart. |
| 3 | **Backup & Restore Verification** | Dump your database, corrupt a random table, and restore from the dump. Measure the time it takes to fully restore (RTO). Verify no data is missing compared to the state before the dump (RPO). |
| 4 | **Token Bucket Rate Limiter** | Implement a custom rate limiter using Redis that allows a burst of 10 requests/second but averages to 5/second. Test with a script that sends rapid bursts. Verify the limiter smooths the traffic rather than blocking strictly. |
| 5 | **Circuit Breaker Integration** | Wrap an external API call (e.g., currency conversion) in a Circuit Breaker. Simulate a timeout. Verify that after 5 failures, the circuit "opens" and immediately returns a cached response without waiting for the timeout. |
| 6 | **Canary Release via Feature Flags** | Create a "New Checkout Flow" feature flag. Roll it out to 10% of users (randomly selected by User ID). Verify that 90% still see the old flow and 10% see the new one. Add a toggle to kill the switch instantly. |
| 7 | **Graceful Degradation Mode** | Implement a "Maintenance Mode" for a specific feature (e.g., Search). When the search engine is down, the UI should show a "Search is offline, try browsing categories" message instead of a spinner or 500 error. |
| 8 | **Idempotency Key Middleware** | Create middleware that checks for an `Idempotency-Key` header on POST requests. If the key has been seen before (within 24 hours), return the original response instead of re-executing the logic. Verify duplicate requests don't create double records. |
| 9 | **Structured JSON Logging** | Replace standard text logs with JSON formatting. Include `level`, `message`, `correlation_id`, `user_id`, and `duration`. Verify you can query these logs using `grep` or a log viewer easily. |
| 10| **Write a Post-Mortem Template** | Create a standardized document for incidents. Include sections for: "Impact," "Timeline," "Root Cause," "Trigger," and "Prevention Actions." Fill it out for a minor bug you fixed recently to practice the workflow. |
