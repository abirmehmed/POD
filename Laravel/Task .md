# Laravel Mastery: 10 Questions & 10 Tasks

## 📘 10 Conceptual Questions
1. **Eloquent Query Building:** When would you use `with()` vs `load()` vs `whereHas()` vs `has()`? Explain the exact SQL difference each generates.
2. **Request Lifecycle:** Trace a typical HTTP request from `public/index.php` to the rendered Blade view. Name the 5 major kernel/middleware layers it passes through.
3. **Service Container:** What’s the difference between `bind()`, `singleton()`, and `alias()`? When should you never use `singleton()`?
4. **Validation Architecture:** Why are `FormRequest` classes preferred over inline `$request->validate()`? How does `authorize()` in a FormRequest change your controller code?
5. **Security Defaults:** Laravel protects against CSRF, XSS, and Mass Assignment out of the box. Explain **how** each protection works under the hood.
6. **Caching Strategy:** When should you use `Cache::remember()` vs `Cache::put()`? How do cache tags work, and when do they become dangerous?
7. **Queues & Jobs:** Why move work to a queue? Explain the difference between `dispatch()`, `dispatchSync()`, and `dispatchAfterResponse()`.
8. **Blade vs Components:** What’s the architectural difference between `@include` and `<x-component>`? How does slot composition prevent "template rot"?
9. **Testing Philosophy:** What’s the difference between Feature and Unit tests in Laravel? Why is `RefreshDatabase` critical in feature tests but harmful in unit tests?
10. **Architecture Patterns:** Explain "Fat Controller, Skinny Model". How do Action Classes, Services, or Repositories fix it without over-engineering?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Resource Controller + Model Binding**<br>Create a `CouponController` with all 7 REST methods. Use implicit route model binding (`Coupon $coupon`). Add `CanViewCoupons` middleware. | Routes resolve automatically. `show()` returns JSON/Blade without manual `find()`. Middleware blocks unauthorized users. |
| 2 | **Advanced Eloquent Aggregations**<br>Add a `reviews` table (`product_id`, `user_id`, `rating`). Define relationships. Write a query that fetches products with `avg_rating` and `reviews_count` using `withAvg()` and `withCount()`. | Single query returns products + aggregated data. No N+1. Blade renders `{{ $product->avg_rating }}` directly. |
| 3 | **Extract FormRequest**<br>Move product creation validation from `AdminProductController@store` into `StoreProductRequest`. Add custom messages + `authorize()` that checks `auth()->user()->isAdmin()`. | Controller `store()` method shrinks to `<5 lines`. Validation errors auto-redirect with old input. Non-admins get 403. |
| 4 | **Reusable Blade Component**<br>Convert your product card HTML into `<x-product-card :product="$product">`. Use named slots for badges (`<x-slot name="badge">`). Add hover scale via Tailwind. | Card renders identically. Badge slot accepts any HTML. Hover animates smoothly. Used in 2+ views without duplication. |
| 5 | **Cache with Invalidation**<br>Wrap category list in `Cache::remember('categories', 3600, ...)`. Create a `CategoryObserver` that calls `Cache::forget('categories')` on create/update/delete. | First load hits DB. Subsequent loads hit cache. Creating a category instantly invalidates cache. Verified via `Cache::has()`. |
| 6 | **Queue a Welcome Email**<br>Create `SendWelcomeEmail` job. Dispatch it on user registration. Configure `database` queue driver. Run `php artisan queue:work` in a second terminal. | Email job appears in `jobs` table. Worker processes it. User registers instantly without email delay. Failed job retries 3x. |
| 7 | **Policy-Based Authorization**<br>Generate `ProductPolicy`. Restrict `update`/`delete` to owner or admin. Use `$this->authorize()` in controller + `@can` in Blade. Add `Gate::before()` for superadmin bypass. | Owner can edit. Other users see 403. Blade hides edit button for non-owners. Superadmin bypasses all checks. |
| 8 | **API Resource + Filtering**<br>Create `/api/products` endpoint. Use `ProductResource` to transform response. Add `?category=electronics&min_price=100` filtering via query scopes. Return paginated JSON. | Returns `{ data: [...], meta: {...} }`. Filtering works without controller bloat. Postman/cURL tests pass. |
| 9 | **Feature Test: Cart Flow**<br>Write `CartTest` with `RefreshDatabase`. Test: guest redirect, valid add-to-cart, stock validation, session persistence. Mock `Product::find()` if needed. | `php artisan test` passes. Asserts cover 4 scenarios. No real DB writes leak between tests. |
| 10| **Extract CartService**<br>Move session logic, stock checks, and total calculation from `CartController` into `CartService`. Inject via constructor. Register in `AppServiceProvider`. | Controller only handles HTTP layer. Service is testable. `php artisan tinker` can instantiate `app(CartService::class)`. |
