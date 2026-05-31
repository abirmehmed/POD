# System Design & Distributed Architecture: Set 1
## Core Patterns, Scaling Fundamentals & Trade-Off Analysis

## 📘 10 Conceptual Questions
1. **CAP Theorem in Practice:** In an e-commerce checkout flow, when do you prioritize Consistency over Availability (e.g., inventory deduction) vs Availability over Consistency (e.g., product views)? How do you safely handle eventual consistency without overselling or double-charging?
2. **Load Balancing Algorithms:** Compare round-robin, least-connections, IP-hash, and weighted routing. When does session stickiness become an anti-pattern, and how do you design stateless services to eliminate it entirely?
3. **Caching Patterns & Stampede Prevention:** Explain cache-aside vs write-through vs write-behind. How do you prevent thundering herds when a popular cache key expires under high concurrency?
4. **Stateless Architecture & Horizontal Scaling:** Why is statelessness the foundation of scale-out? How do you externalize sessions, carts, locks, and rate counters without introducing single points of failure?
5. **Vertical vs Horizontal Scaling Inflection Point:** When does "bigger server" stop working? What architectural signals (CPU saturation, connection limits, deployment downtime, single-region risk) force you to scale out instead?
6. **API Gateway vs Direct Routing:** When do you need an intermediary for auth, rate limiting, response transformation, and request routing? When does a gateway become an unnecessary bottleneck?
7. **Rate Limiting Algorithms at Scale:** Compare fixed window, sliding window, and token bucket. How do you implement per-user/IP limits with Redis without causing high latency or memory bloat?
8. **Graceful Degradation vs Silent Failure:** How do you design fallback responses when critical dependencies (payment gateways, search, email) fail? What's the difference between degrading gracefully and hiding errors from users?
9. **Observability Triad Foundations:** Metrics vs Logs vs Traces. What should trigger a page vs what should just be recorded? Why is "alert fatigue" a symptom of poor signal-to-noise ratio, not bad ops?
10. **Architectural Trade-Off Documentation:** How do you evaluate and record decisions without analysis paralysis? What makes a trade-off sustainable vs a technical debt trap, and how do you revisit stale decisions?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Cache-Aside + Request Coalescing**<br>Wrap product catalog in cache-aside pattern. Implement mutex/cache lock to allow only one origin fetch per key expiry. Measure HIT/MISS ratio under concurrent load. | Cache invalidates correctly on update. Concurrent requests share a single origin fetch. HIT ratio >80%. No cache stampede under 50+ RPS test. |
| 2 | **Externalize Session to Redis**<br>Switch Laravel session driver from `file`/`database` to `redis`. Verify session persistence across multiple local app instances. Simulate server restart with zero session loss. | Sessions survive instance bounce. Concurrent requests don't collide. Session data serializes/deserializes correctly. `php artisan session:table` no longer required. |
| 3 | **Sliding Window Rate Limiter**<br>Build custom middleware using Redis sorted sets. Track requests per user/IP over rolling 60s window. Apply `429` with `Retry-After`. Test with concurrent script. | Accurate counting across boundaries. Returns 429 consistently. Redis keys auto-expire. No memory growth. Logs capture abuse patterns. |
| 4 | **Health Checks + Connection Draining**<br>Create `/health` endpoint returning JSON status. Configure reverse proxy (Nginx/Traefik/Laravel Sail) to route traffic, drain connections on shutdown, handle rolling deploys without 502s. | Health returns 200/503 correctly. Zero dropped requests during deploy. Connections drain gracefully. Proxy respects health status. |
| 5 | **Circuit Breaker + Fallback Response**<br>Wrap external API call (e.g., shipping rates, currency conversion) in circuit breaker pattern. Track failures, open circuit after threshold, return cached/fallback data. Log state transitions. | Circuit opens/closes correctly. Fallback data served during outage. No cascading timeouts. State transitions logged & auditable. |
| 6 | **Structured Logging + Correlation IDs**<br>Add `X-Request-ID` middleware. Propagate across web → queue → DB. Format logs as JSON. Verify you can trace a single request end-to-end using only the ID. | Trace spans link correctly. Logs queryable by ID. Business metadata attached. Performance impact <2%. CLI/tooling can filter by ID. |
| 7 | **Write an ADR for a Scaling Decision**<br>Document a real trade-off in MultiMart (e.g., "Redis-backed sessions vs DB sessions", "Cache strategy for product listings"). Use standard template: context, options, decision, consequences, status. | ADR follows standard format. Clear trade-offs documented. Status tracked. Team/future-you can reference it. Linked from README. |
| 8 | **Stateless Cart Service Design**<br>Refactor cart to store data in Redis/hash instead of DB/session. Ensure multiple app instances can read/write the same cart. Verify atomic quantity updates and TTL cleanup. | Cart survives instance restart. Concurrent updates don't corrupt data. Expired carts auto-purge. Zero session dependency. |
| 9 | **Graceful Degradation Simulation**<br>Disable a critical service (DB read replica, cache, or external API). Verify UI falls back to cached data, queued actions, or simplified layout. Measure user impact vs hard failure. | System degrades gracefully under each failure. No data loss. Fallbacks activate automatically. Post-drill report lists 3 improvements implemented. |
| 10| **Horizontal Scaling Stress Test**<br>Run 2+ app instances behind load balancer/reverse proxy. Simulate 200+ concurrent requests. Monitor CPU, memory, connection count, and response time. Identify first bottleneck. | Bottleneck documented (DB, cache, app, or network). Response time stays <500ms at target RPS. No 502/504 errors. Scaling plan drafted based on findings. |
