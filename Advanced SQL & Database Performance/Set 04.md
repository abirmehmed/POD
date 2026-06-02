# Advanced SQL & Database Performance: Set 4 
## Scaling, Replication, High Availability & Capacity Planning

## 📘 10 Conceptual Questions
1. **Read Replicas & Replication Lag:** How does asynchronous replication cause "read-your-own-write" anomalies? When should you force critical reads to the Primary database, and what is the performance cost of doing so?
2. **Connection Pooling Mechanics:** Why do modern web frameworks (like Laravel) exhaust database connections under high concurrency? How do connection poolers (PgBouncer, ProxySQL) solve this by multiplexing thousands of app connections onto a few persistent database connections?
3. **Sharding vs. Partitioning:** What is the fundamental difference between Horizontal Partitioning (within one database) and Sharding (across multiple databases)? When does sharding become an operational nightmare that you can never undo?
4. **The Cross-Shard Join Problem:** Why are `JOIN`s across different shards physically impossible or incredibly slow? How do you design your schema (using "co-location" or "lookup tables") to avoid needing cross-shard joins?
5. **High Availability (HA) & Split-Brain:** How do automated failover systems (like Patroni or AWS Multi-AZ) detect a dead primary? What is the "split-brain" problem, and how do quorum-based consensus algorithms (like etcd/Zookeeper) prevent it?
6. **Database Proxies (ProxySQL / Vitess):** What role does a database proxy play in a scaled architecture? How does it handle query routing, caching, and connection pooling transparently to the application?
7. **The `max_connections` Ceiling:** Why is simply adding more CPU/RAM useless once you hit the database's connection limit? How do you calculate the optimal `max_connections` based on available RAM and thread stack size?
8. **Scaling Write Throughput:** Read replicas solve read scaling, but how do you scale *writes*? Explain how sharding, partitioning, and asynchronous queueing (offloading writes to a background job) increase write capacity.
9. **Non-Blocking Schema Changes:** How do tools like `gh-ost` (GitHub Online Schema Tool) or `pt-online-schema-change` add columns to billion-row tables without locking the table or blocking `INSERT/UPDATE` operations?
10. **Capacity Planning & IOPS:** How do you know when your disk is the bottleneck? Explain IOPS (Input/Output Operations Per Second) vs. Throughput (MB/s), and how to use monitoring to predict when you need to upgrade your storage class.

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Configure Read/Write Splitting** | Set up a Primary and a Read Replica (use local Docker containers if needed). Configure Laravel's `database.php` to route writes to primary and reads to replica. Verify using query logs. |
| 2 | **Handle Replication Lag** | Simulate a heavy write, then immediately read the data. Observe the lag. Implement a middleware or service logic that forces reads to the Primary for 5 seconds after a user performs a write. |
| 3 | **Implement Connection Pooling** | Set up PgBouncer (PostgreSQL) or ProxySQL (MySQL) in Docker. Point Laravel to the pooler instead of the raw DB. Simulate 500 concurrent connections and verify the DB only sees the pool's max connections. |
| 4 | **Table Partitioning** | Create a `logs` or `orders` table with 100,000+ rows. Partition it by `created_at` (Range Partitioning). Run a query filtering by date and use `EXPLAIN` to prove "Partition Pruning" is happening. |
| 5 | **Design a Sharding Strategy** | Draft a sharding plan for a multi-tenant SaaS app. Choose a "Shard Key" (e.g., `tenant_id`). Document how you will route queries, handle global lookups (like user login), and manage the shard directory. |
| 6 | **Simulate Failover** | Use a tool like Docker Compose with a primary/replica setup. Kill the primary container. Measure exactly how many seconds the application experiences errors before the replica is promoted or traffic is rerouted. |
| 7 | **Non-Blocking Schema Migration** | Simulate a massive table. Run an `ALTER TABLE` to add a column. Note the lock time. Then, simulate the `gh-ost` workflow (create ghost table, sync triggers, atomic swap) to achieve the same result without locking. |
| 8 | **Offload Heavy Analytics** | Identify a slow, heavy reporting query in MultiMart. Move it to execute exclusively on the Read Replica (or a separate analytics database). Verify the primary DB's CPU usage drops during report generation. |
| 9 | **Database Stress Testing** | Use a tool like `sysbench` or `JMeter` to bombard your database with read/write queries. Monitor CPU, Memory, and Disk I/O. Identify the exact breaking point (where latency spikes or connections refuse). |
| 10| **Write a Database Scaling Runbook** | Create `RUNBOOK_DB_SCALING.md`. Document step-by-step actions for: "CPU at 90%", "Too many connections", "Disk space critical", and "Replication lag > 60s". Include commands to check and mitigate. |
