# Laravel Deep Dive: 10 Questions & 10 Tasks

## 📘 10 Conceptual Questions
1. **Database Performance & Indexing:** How do database indexes work in Laravel? Explain when to add composite indexes vs single-column indexes, and how to use `EXPLAIN` to optimize slow Eloquent queries.
2. **Eloquent Collections vs Query Builder:** What’s the difference between executing logic in SQL (`where`, `groupBy`, `select`) vs PHP collections (`filter`, `map`, `reduce`)? When does pulling data into memory become a performance risk?
3. **Casts vs Accessors/Mutators:** Laravel moved from old accessor/mutator syntax to `Casts`. Explain how `HasAttributes` works, when to use `AsEnum::class`, `AsArrayObject::class`, or custom casts, and why they’re safer than raw PHP methods.
4. **Routing Architecture & Caching:** How do route groups, middleware stacks, and subdomain routing compile? Explain why `php artisan route:cache` speeds up production but breaks during development, and how fallback routes work.
5. **Real-time Broadcasting:** How does Laravel’s broadcasting system work (Reverb/Pusher)? Explain the difference between public, private, and presence channels, and how to authorize subscriptions securely.
6. **Testing External Dependencies:** How do you test code that calls third-party APIs or mail services? Explain `Http::fake()`, `Notification::fake()`, `Mail::fake()`, and when to use mocking vs fake drivers.
7. **Configuration & Environment Safety:** How does config caching work? Why should you never call `env()` outside of `config/` files, and how do you structure multi-environment deployments without leaking secrets?
8. **Notification Architecture:** When should you use `Notification` vs `Mail` vs `Event`? Explain how to queue notifications, customize channels (database, mail, slack, sms), and handle failed deliveries with retry policies.
9. **Stateful Frontend Integration (Livewire/Inertia):** How does Laravel handle reactive UIs? Compare Livewire’s component lifecycle with Inertia’s SSR/client routing, and explain when to choose one over a traditional Blade + AJAX approach.
10. **Package Development & Extensibility:** How do you create a reusable Laravel package? Explain service provider registration, facade creation, config publishing, and how to avoid namespace collisions in vendor code.

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Query Optimization & Indexing**<br>Enable `DB::listen()` or Laravel Debugbar. Identify 2 slow queries. Add proper indexes via migration, rewrite with explicit `select()`, and verify `EXPLAIN` shows `Using index` or reduced rows. | Query time drops >50%. No `SELECT *`. Indexes documented in migration comments. |
| 2 | **Collection Pipeline Refactor**<br>Find a controller method using `foreach` to calculate totals, group data, or filter arrays. Replace it entirely with Eloquent/Collection methods (`map`, `filter`, `groupBy`, `reduce`). | Output remains identical. Code lines reduced. Zero manual loops. Passes existing tests. |
| 3 | **Custom Cast Implementation**<br>Create a `CurrencyCast` that stores values as integers (cents) but formats as `$19.99` in Blade. Apply to `Product::price`. Ensure form validation and mass assignment work seamlessly. | DB stores `1999`. Blade shows `$19.99`. Form submits strings/floats correctly. No manual conversion in controllers. |
| 4 | **Route Structure & Caching**<br>Split routes into `routes/web.php`, `routes/admin.php`, `routes/api.php`. Use `Route::prefix()`, `Route::as()`, and group middleware. Run `php artisan route:cache` and verify no resolution errors. | Admin/API routes isolated. Prefixes/names work. Cache command succeeds. `php artisan route:list` shows clean hierarchy. |
| 5 | **Real-time Low Stock Alert**<br>Create a `StockDropped` broadcast event. Set up a private `admin-alerts` channel. Use Reverb or Pusher to show a live browser toast when inventory falls below threshold. | Event fires on update. Admin subscribed channel receives payload. Toast appears without page reload. Unauthorized users blocked. |
| 6 | **HTTP Test with API Mock**<br>Write a feature test for a "price sync" feature that calls an external API. Use `Http::fake()` to return mock JSON. Assert correct DB updates, error handling, and retry logic. | Test passes offline. Fake response matches structure. DB state verified. No real network calls made. |
| 7 | **Multi-Environment Config**<br>Create `.env.staging` and `.env.production`. Move environment-specific settings (mail driver, cache prefix, debug mode) into `config/`. Verify `php artisan config:show` reflects each env. | `env()` never called outside config. Staging/production configs differ safely. No hardcoded secrets in repo. |
| 8 | **Multi-Channel Notification**<br>Create a `LowStockNotification` that emails admins AND logs to the `notifications` table. Queue it, add a 3-retry policy, and test failed delivery handling with `Notification::fake()`. | Notification sends on trigger. Database record created. Retry policy active. Fake test captures both channels. |
| 9 | **Livewire Live Search**<br>Build a `LiveProductSearch` component that filters products as the user types (debounced). Use `wire:model.live` and optimize with `lazy` loading. No full page reloads. | Results update instantly. Debounce prevents excessive queries. Pagination/filter state preserved. Works on mobile. |
| 10| **Local Package Extraction**<br>Extract your `CartService` into `packages/multimart/cart`. Register via Composer path repository, create a `Cart` facade, and use it in controllers without breaking existing code. | `composer.json` includes path repo. Facade resolves correctly. Service tests pass. Controllers unchanged except import. |
