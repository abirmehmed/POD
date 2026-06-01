# System Design & Distributed Architecture: Set 2
## Data & Storage Scaling, Consistency Models & Query Optimization

## 📘 10 Conceptual Questions
1. **Read/Write Splitting & Replication Lag:** How do you handle eventual consistency when reads hit replicas that are milliseconds behind? When should you force primary reads, and what performance trade-offs does that introduce?
2. **CQRS vs Traditional CRUD:** When does separating read and write models actually improve throughput vs adding unnecessary complexity? How do you keep them in sync without tight coupling or double-writes?
3. **Connection Pooling vs Direct Connections:** Why do modern frameworks struggle under high concurrency without pooling? How do poolers, persistent connections, and connection limiters prevent database exhaustion?
4. **Sharding vs Partitioning:** What’s the architectural difference? When does horizontal sharding become an operational nightmare, and how do you route queries without a centralized index or cross-shard joins?
5. **Cache Warming vs Lazy Loading:** When should you pre-populate caches vs fetch-on-demand? How do you prevent cold-start latency, cache stampedes, and memory bloat during traffic spikes?
6. **Strong vs Eventual Consistency:** How do you choose between them for different data types (inventory, orders, analytics, user profiles)? What compensation patterns fix consistency drift without manual intervention?
7. **Indexing Strategy & Query Plans:** How do composite, covering, and partial indexes change execution plans? When does over-indexing hurt write throughput more than it helps reads?
8. **Zero-Downtime Schema Migrations:** How do you safely add columns, rename tables, or backfill data on large tables without locking or breaking active queries? What’s the expand-contract pattern?
9. **Data Lifecycle & Archival:** When should data move from hot storage to warm/cold tiers? How do you implement tiered retention without breaking referential integrity or audit compliance?
10. **Database Observability:** Beyond `slow_query_log`, what metrics actually predict database failure? How do you correlate connection saturation, lock contention, and replication lag before users notice?

---

## ️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Read/Write Routing + Lag Handling**<br>Configure database routing (reads → replica, writes → primary). Simulate replication delay, verify stale reads, implement primary fallback for critical paths. | Reads route to replica, writes to primary. Critical checkout forces primary. Stale data detected & handled. Query logs confirm routing. |
| 2 | **CQRS Read Model Implementation**<br>Create a denormalized read table/view for product listings. Bypass ORM for queries. Sync writes via events/jobs. Measure read latency vs direct ORM queries. | Read queries execute in <50ms. Zero joins on hot path. Sync job runs idempotently. Write path unaffected. |
| 3 | **Connection Pool Stress Test**<br>Simulate 100+ concurrent workers/requests. Monitor DB connection count. Implement pooling/limiting strategy. Verify no "too many connections" errors under load. | Connection count stays within limits. No exhaustion errors. Pool recycles connections efficiently. Documented max concurrency threshold. |
| 4 | **Zero-Downtime Column Addition**<br>Add a new column to a high-traffic table using expand-contract: add nullable → backfill → enforce NOT NULL. Verify zero table locks or query failures during rollout. | Migration completes without downtime. Backfill runs in chunks. Final constraint applied safely. Rollback path documented. |
| 5 | **Composite Index & EXPLAIN Optimization**<br>Identify a slow query. Add optimal composite index. Run `EXPLAIN` before/after. Verify index usage, eliminated filesort/temp table, and reduced rows examined. | `EXPLAIN` shows `Using index` or reduced scan rows. Query time drops >50%. No regression on write performance. Index documented. |
| 6 | **Cache Warming Pipeline**<br>Build scheduled job to pre-load top products/categories into Redis. Measure cold-start vs warm response time. Implement TTL refresh to prevent expiration stampedes. | Warm response time <30ms. Cache HIT ratio >85%. No stampede on expiry. Job runs within maintenance window. |
| 7 | **Eventual Consistency Compensation**<br>Simulate out-of-sync inventory (cache vs DB). Build reconciliation job that detects drift, corrects values, and logs anomalies. Run on schedule. | Drift corrected automatically. No overselling. Reconciliation idempotent. Anomaly log alerts on persistent sync failure. |
| 8 | **Cursor-Based Data Archival**<br>Archive records >1 year old using `chunkById()` or `cursor()`. Move to cold table. Verify memory stays flat during execution. Keep audit trail intact. | Memory usage constant. Cold table populated correctly. Original table shrinks. Command resumable & idempotent. |
| 9 | **Replication Lag Detection & Routing**<br>Inject artificial replication delay. Implement lag detection middleware. Route reads to primary when lag > threshold. Log warnings for monitoring. | Lag detected accurately. Critical reads auto-route to primary. Non-critical reads tolerate lag. Alerts trigger on sustained delay. |
| 10| **DB Performance Dashboard**<br>Set up monitoring for connections, slow queries, lock waits, replication status, and cache hit ratio. Configure alert thresholds. Verify dashboard reflects real-time state. | Metrics visible & accurate. Alerts fire before user impact. No false positives. Runbook links to dashboard for triage. |
