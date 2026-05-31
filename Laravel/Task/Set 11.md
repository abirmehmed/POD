# Laravel Real-Time, Payments & Resilient Workflows: 10 Questions & 10 Tasks

## 📘 10 Conceptual Questions
1. **WebSockets vs Polling vs SSE:** When should you use Laravel Reverb/Broadcasting vs HTTP polling vs Server-Sent Events? What are the connection limits, scaling implications, and fallback strategies for each?
2. **Subscription Billing Lifecycles:** How do you handle proration, trial periods, dunning (failed payment recovery), and grace periods in Laravel Cashier? What happens when a payment method expires mid-cycle?
3. **Job Chaining vs Batching vs Workflow Engines:** Compare `Bus::chain()`, `Bus::batch()`, and dedicated workflow engines (Temporal/Cadence). When does a simple chain become a nightmare, and how do you manage compensating transactions (rollbacks)?
4. **Idempotency in Payment Webhooks:** Why must webhook handlers be idempotent? How do you implement idempotency keys, deduplication caches, and safe retries without double-charging or double-shipping?
5. **Rate Limiting at Scale:** How does Laravel's rate limiter work with Redis? Explain sliding window vs fixed window algorithms, per-user vs per-IP limits, and how to prevent abuse without blocking legitimate traffic.
6. **Async File Processing Pipelines:** How do you handle large file uploads, transformations (image resizing, PDF generation), and virus scanning without blocking web requests? Explain queues, temporary storage, and cleanup strategies.
7. **Multi-Tenant Data Isolation:** Compare schema-per-tenant vs row-level security (`tenant_id`) vs database-per-tenant. How do you enforce isolation at the ORM, query, and cache levels to prevent data leaks?
8. **Distributed Tracing & Observability:** How do you trace a request across web → queue → external API → database? Explain OpenTelemetry integration, span context propagation, and how to correlate logs across async boundaries.
9. **Feature Flags & Progressive Delivery:** How do you implement database-backed feature flags with percentage rollouts, user segmentation, and instant rollback? Explain flag pollution prevention and cleanup strategies.
10. **Circuit Breakers & Graceful Degradation:** When external services (payment gateways, shipping APIs, search engines) fail, how do you prevent cascading failures? Explain circuit breaker patterns, fallback responses, and health check routing.

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Real-Time Stock Alerts** | Implement Reverb/WebSockets to broadcast low-stock alerts to admin dashboard. Use private channels, auth via middleware, and fallback to polling if WS fails. Verify real-time updates without page refresh. |
| 2 | **Subscription Proration Flow** | Integrate Laravel Cashier. Handle plan upgrades/downgrades with automatic proration. Test mid-cycle changes, invoice generation, and payment method updates. Verify webhooks sync correctly. |
| 3 | **Compensating Transaction Workflow** | Create a job chain: `ReserveInventory → ProcessPayment → CreateShipment`. If payment fails, trigger `ReleaseInventory` job. Implement saga pattern with state tracking and manual retry endpoint. |
| 4 | **Idempotent Webhook Handler** | Build `/webhooks/stripe` that processes `payment_intent.succeeded`. Use idempotency key cache, signature verification, and `Cache::add()` to prevent duplicates. Test with replayed payloads. |
| 5 | **Redis Rate Limiting Setup** | Configure rate limiter with Redis driver. Set `throttle:60,1` for API, custom `throttle:10,1` for login. Test with concurrent requests. Verify `429` responses, retry headers, and no memory leaks. |
| 6 | **Async File Processing Pipeline** | Create upload endpoint that queues image resizing, watermarking, and CDN sync. Use `Storage::temporaryUrl()` for preview. Implement cleanup job for orphaned files. Monitor queue memory/CPU. |
| 7 | **Tenant Data Isolation Enforcement** | Implement middleware that sets `tenant_id` on all queries. Use global scope or tenancy package. Test cross-tenant query attempts. Verify cache keys are prefixed. Document isolation guarantees. |
| 8 | **Distributed Tracing Integration** | Install OpenTelemetry PHP SDK. Configure trace context propagation across HTTP → Queue → DB. Verify spans link correctly in Jaeger/Zipkin. Add business metadata (order_id, user_id) to spans. |
| 9 | **Database-Backed Feature Flags** | Create `FeatureFlag` model with `key`, `enabled`, `percentage`, `user_segment`. Implement `Flag::check('new_checkout', $user)`. Add admin UI. Test rollout consistency and instant toggle. |
| 10| **Circuit Breaker for External API** | Wrap shipping rate API call in circuit breaker pattern. Track failure count, open circuit after 5 failures, return cached/fallback rates. Auto-close after recovery timeout. Log state transitions. |
