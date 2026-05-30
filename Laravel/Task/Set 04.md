# Laravel Professional: 10 Questions & 10 Tasks

## 📘 10 Conceptual Questions
1. **Database Constraints & Migrations:** How do `foreignIdConstrained()`, `cascadeOnDelete()`, and `nullOnDelete()` change referential integrity? When should you avoid database-level constraints in favor of application-level checks?
2. **Local Scopes vs Global Scopes:** How do `scopeActive()`, `scopeForUser()`, etc., differ from global scopes? How do you chain them safely, and what happens when you combine them with `whereHas()` or `orWhere()`?
3. **Rate Limiting & Throttling:** How does Laravel's rate limiter work under the hood? Explain how to configure custom limits per route/user, how cache drives affect throttling, and what happens when limits are exceeded.
4. **Custom Validation Rules:** When should you create a `Rule` class vs inline validation vs `FormRequest`? How do you handle async validation (e.g., checking username availability via AJAX) without compromising security?
5. **Session Architecture & Security:** How does Laravel handle session fixation, cookie encryption, and garbage collection? When should you use `database` vs `redis` vs `file` drivers, and how do `secure`/`same_site` cookies impact auth?
6. **Factory & Seeder Design:** How do you structure factories that create related models (`has()`, `for()`, `state()`, `sequence()`)? When should you use `DatabaseSeeder` vs model factories vs raw SQL inserts?
7. **Queue Worker Management:** How do you monitor, retry, and fail queue jobs? Explain `--tries`, `--backoff`, `FailedJobProvider`, and how to safely handle poison messages without blocking the worker.
8. **API Versioning Strategy:** How do you version APIs in Laravel? Compare URI versioning (`/v1/products`), header versioning, and content negotiation. How do you maintain backward compatibility without duplicating business logic?
9. **CORS & Cross-Origin Security:** How does Laravel's CORS middleware work? When should you configure allowed origins, methods, and headers? What's the difference between CORS policies and Content Security Policy (CSP)?
10. **Telescope vs Debugbar:** How does Laravel Telescope differ from local debugging tools? When should you enable it in staging/production, and how do you safely filter sensitive data (passwords, tokens, PII)?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Local Scopes & Query Chaining**<br>Add `scopePublished()`, `scopeForCategory()`, `scopePriceRange()` to `Product`. Chain them in a controller method that returns paginated results. Verify no N+1 and correct SQL `WHERE` clauses. | `Product::published()->forCategory($id)->priceRange(10,100)->paginate()` works. Query log shows combined conditions. Blade renders correctly. |
| 2 | **Custom Validation Rule**<br>Create `SlugAvailable` rule that checks uniqueness across soft-deleted records. Apply to product creation form. Test with valid/invalid slugs and edge cases (trailing spaces, case variations). | Rule passes unique active slugs. Fails on duplicates/soft-deleted slugs. Normalizes input. Form displays clear error message. |
| 3 | **Rate Limiting Configuration**<br>Apply `throttle:10,1` to login route. Create custom `api:100,1` middleware group for product search. Test with rapid requests using a script or browser dev tools. | Login locks after 10 attempts/min. Search respects 100 req/min. `429` status returned with `Retry-After` header. Cache driver tracks limits correctly. |
| 4 | **Advanced Factories**<br>Create `OrderFactory` with `forProduct()`, `hasItems()`, `state('paid')`, and `sequence()`. Seed 50 orders with varying statuses. Verify counts match and relationships persist. | `Order::factory()->count(50)->create()` generates valid DB records. Related items exist. States distribute correctly. Seeder runs idempotently. |
| 5 | **Queue Failure & Retry Management**<br>Configure `QUEUE_FAILED_DRIVER=database`. Create a job that fails 3 times then marks as failed. Write artisan command to retry failed jobs by tag or date range. | Failed jobs store in `failed_jobs` table. `php artisan queue:retry` works selectively. No poison messages block worker. Logs capture failure reasons. |
| 6 | **API Versioning Implementation**<br>Create `/api/v1/products` and `/api/v2/products`. V2 adds `discounted_price` field. Use route groups, version-specific resource transformers, and backward-compatible responses. | Both versions return valid JSON. V2 includes new field without breaking V1 clients. Controllers share base logic. Postman tests pass for both. |
| 7 | **CORS Configuration**<br>Configure CORS to allow `http://localhost:3000` for API routes. Restrict methods to GET/POST. Test with browser `fetch()` vs Postman/cURL. Verify preflight `OPTIONS` handling. | Browser requests succeed with proper headers. `Access-Control-Allow-Origin` matches exactly. `OPTIONS` returns 204. Invalid origins blocked. |
| 8 | **Telescope Setup & Filtering**<br>Install/configure Telescope. Filter sensitive data (passwords, tokens, session data). Set up daily pruning via scheduler. Verify it captures queries, exceptions, and mail. | Telescope UI loads in staging. Sensitive fields masked. `telescope:prune` runs automatically. No production data leakage. |
| 9 | **Session Security Hardening**<br>Switch session driver to `database`. Create migration. Configure `session_secure_cookie`, `session_same_site='lax'`, and garbage collection. Verify persistence across requests. | Sessions store in `sessions` table. Cookies set with `Secure; SameSite=Lax`. GC runs via schedule. Auth persists after browser restart. |
| 10| **Audit Trail Implementation**<br>Create `AuditLog` model/migration. Use model events or observers to log `created`, `updated`, `deleted` actions with user ID, old/new values, and IP. Query recent audits. | Logs capture all mutations. JSON diffs stored efficiently. Admin view shows timeline. Performance impact <2% on write operations. |
