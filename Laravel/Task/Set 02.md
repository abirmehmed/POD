# Laravel Advanced: 10 Questions & 10 Tasks

## 📘 10 Conceptual Questions
1. **Database Transactions & Concurrency:** When should you use `DB::transaction()`? How do `lockForUpdate()` and `forUpdate()` prevent race conditions in inventory or checkout flows?
2. **Polymorphic Relationships:** Explain `morphTo()`, `morphMany()`, and `morphedByMany()`. When is polymorphism better than multiple foreign keys, and when does it become an anti-pattern?
3. **Global Scopes & Soft Deletes:** How do global scopes work under the hood? How does `SoftDeletes` integrate with queries, and how do you bypass or modify it safely?
4. **API Authentication (Sanctum vs Passport):** When should you use Sanctum vs Passport? Explain SPA authentication vs token-based API authentication in Laravel's security model.
5. **Events & Listeners vs Observers:** What’s the architectural difference? When should you use domain events vs model observers? How do you handle failed listeners or queue them?
6. **Custom Middleware & Request Pipeline:** How does middleware execute before/after a request? Explain `terminate()` middleware and when you’d use it for logging or cleanup.
7. **File Storage & Media Strategy:** Compare `local`, `public`, `s3` disks. How does `Storage::put()` vs `putFile()` differ? How do you handle temporary URLs and secure file access?
8. **Cursor Pagination vs Offset:** Why is cursor pagination faster for large datasets? How does it change your Eloquent queries and frontend infinite scroll implementation?
9. **Service Providers & Bootstrapping:** What’s the difference between `register()` and `boot()`? When should you defer a provider, and how does it impact application startup time?
10. **Error Handling & Logging Strategy:** How does Laravel’s exception handler work? When should you use `report()` vs `render()`? How do you structure logs for production debugging without leaking sensitive data?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Transaction & Locking**<br>Implement a "purchase" flow that decrements product stock safely. Use `DB::transaction()` and `lockForUpdate()`. Test with concurrent requests using a script or queue stress test. | Stock never goes negative under concurrent purchases. Deadlocks handled gracefully. Logs show transaction boundaries. |
| 2 | **Polymorphic Comments**<br>Add a `comments` table that can attach to `Product`, `Order`, or `User`. Implement a `Commentable` trait. Query latest comments per model type. | Single `Comment` model works across 3+ models. `@forelse` in Blade renders mixed comments. No duplicate tables. |
| 3 | **Global Scope for Active Records**<br>Create a global scope that automatically filters `where('is_active', true)` on all `Product` queries. Show how to bypass it with `withoutGlobalScope()`. | All product lists auto-filter. Admin panel bypasses scope. Query log confirms scope application. |
| 4 | **Sanctum API Tokens**<br>Secure `/api/products` with Sanctum. Create a token generation endpoint, protect routes with `auth:sanctum`, and test with Postman/cURL. | Unauthenticated requests return 401. Token header authenticates successfully. SPA/CSRF flow works if tested in browser. |
| 5 | **Event/Listener Architecture**<br>Fire an `OrderPlaced` event on checkout. Create a queued `SendOrderConfirmation` listener and a synchronous `UpdateInventory` listener. | Event fires once. Email job queues correctly. Inventory updates immediately. Failed listener retries 3x. |
| 6 | **Custom Middleware**<br>Create `TrackPageViews` middleware that logs visited routes and user IP to a database table. Apply it globally but exclude admin routes. | Admin routes untouched. Frontend routes log correctly. Performance impact <5ms per request. |
| 7 | **Secure File Uploads**<br>Replace `public` disk for sensitive documents (e.g., invoices) with `private` storage. Generate temporary signed URLs that expire in 1 hour. | Files inaccessible via direct URL. Temporary URL works for 60 mins, then 403s. `Storage::url()` never exposes path. |
| 8 | **Cursor Pagination Implementation**<br>Replace standard pagination on a large product list with cursor pagination. Update frontend to handle `next_cursor` for infinite scroll. | No `OFFSET` in SQL queries. Scroll loads smoothly. Back button works. Query count drops significantly. |
| 9 | **Defer a Service Provider**<br>Extract heavy initialization logic (e.g., third-party SDK setup) into a custom service provider. Use `$defer = true`. Verify it only loads when resolved. | `register()` runs immediately, `boot()` delays until service call. Startup time improves. `php artisan about` shows deferred status. |
| 10| **Custom Artisan Command**<br>Create `php artisan multimart:sync-prices` that fetches external API data, updates products in chunks, and logs progress. Add `--force` flag and confirmation prompt. | Runs without timeout. Handles partial failures. Progress bar/logs visible. Idempotent (safe to rerun). |
