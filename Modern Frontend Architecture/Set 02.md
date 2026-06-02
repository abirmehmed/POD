# Modern Frontend Architecture: Set 2
## Advanced Reactivity, Performance & Data Fetching

## 📘 10 Conceptual Questions
1. **Reactivity Models Compared:** How do Virtual DOM diffing (React), Proxy-based reactivity (Vue 3), and fine-grained reactivity (Solid/Svelte) differ under the hood? What are the performance trade-offs of each when rendering large, frequently updating lists?
2. **Memoization: Necessity vs. Premature Optimization:** When should you actually use `useMemo`/`useCallback` (React) or `computed` (Vue)? Why can over-memoizing actually *degrade* performance due to the overhead of dependency tracking and closure creation?
3. **Optimistic UI Updates:** How do you make an application feel instant (e.g., "liking" a post or adding to cart) before the server responds? What is the rollback strategy if the API request ultimately fails?
4. **The Fetch Waterfall Anti-Pattern:** What causes a "waterfall" of network requests (e.g., Component A fetches user, then Component B fetches user's orders)? How do you restructure code to fetch data in parallel or at a higher level in the component tree?
 to Revalidate (SWR):** How do modern data-fetching libraries (TanStack Query, SWR, Pinia Colada) implement SWR? Why is serving stale data instantly while fetching fresh data in the background the gold standard for perceived performance?
6. **Virtualization (Windowing) for Large Lists:** Why does rendering 10,000 DOM nodes crash or freeze a browser, even if the data is already in memory? How does virtualization solve this by only rendering what is visible in the viewport?
7. **Bundle Splitting & Lazy Loading:** How does dynamic `import()` work to split a massive JavaScript bundle into smaller chunks? What is the impact of route-level vs. component-level splitting on Core Web Vitals (specifically LCP and INP)?
8. **Debouncing vs. Throttling:** What is the exact mechanical difference between these two techniques? When should you use debounce (e.g., search inputs) vs. throttle (e.g., scroll events, window resizing)?
9. **Frontend Memory Leaks:** How do Single Page Applications (SPAs) leak memory? Explain the dangers of uncleared `setInterval`s, un-removed `window.addEventListener`s, and un-aborted `fetch` requests when components unmount.
10. **Perceived Performance & Skeleton Screens:** Why is a well-designed skeleton screen often better for user experience than a spinning loader? How do you prevent Cumulative Layout Shift (CLS) while async data is loading?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Optimistic UI Implementation**<br>Build an "Add to Cart" or "Toggle Favorite" button. Update the UI state instantly. If the API call fails, revert the state, show an error toast, and log the failure. | UI responds in <50ms. Network tab shows the API call. Simulated API failure correctly reverts the UI and notifies the user. |
| 2 | **Eliminate a Fetch Waterfall**<br>Identify or create a component tree where Child B waits for Parent A to finish fetching before it starts. Refactor to use `Promise.all`, a higher-level data fetcher, or a library like TanStack Query to run them in parallel. | Network tab shows requests firing simultaneously. Total load time for the view drops significantly. |
| 3 | **Virtualized List Rendering**<br>Render a list of 5,000 mock products using a virtualization library (e.g., `@tanstack/react-virtual` or `vue-virtual-scroller`). Inspect the DOM to verify only ~20-30 items are rendered at a time. | DOM node count remains low (<100) regardless of total data size. Scrolling is smooth at 60fps. |
| 4 | **Custom Debounce Composable/Hook**<br>Create a reusable `useDebounce` function. Apply it to a live search input. Verify via the Network tab that typing "apple" rapidly only triggers *one* API request after the user stops typing. | Network requests are strictly limited. The debounced value updates correctly after the delay. Code is reusable across components. |
| 5 | **Route-Level Code Splitting**<br>Configure your router (Vue Router / React Router) to lazy-load heavy pages (e.g., Admin Dashboard, Checkout) using dynamic imports. Verify in the Network tab that these JS chunks are only downloaded when navigating to that route. | Initial bundle size shrinks. Navigating to the route triggers a new network request for the chunk. |
| 6 | **SWR Caching Configuration**<br>Configure a data-fetching library to cache a "Product Details" query for 5 minutes. Navigate away, then return. Verify the stale data renders instantly, and a background request fetches fresh data (visible in Network tab). | Instant render on return. Background fetch occurs silently. UI updates seamlessly when new data arrives. |
| 7 | **Memory Leak Prevention**<br>Create a component that sets up a `setInterval` or `window.addEventListener('resize', ...)`. Ensure the cleanup function (`onUnmounted` or `useEffect` return) properly clears the interval/removes the listener. | Component can be mounted/unmounted 100 times without increasing memory usage or triggering ghost events. |
| 8 | **Image Optimization & CLS Prevention**<br>Replace standard `<img>` tags with optimized equivalents (e.g., Next.js `<Image>`, Nuxt `<NuxtImg>`, or manual `srcset` + `width`/`height` attributes). Add `loading="lazy"`. Verify zero layout shift in DevTools. | Images load in modern formats (WebP/AVIF). Layout does not jump when images load. Lighthouse CLS score is 0. |
| 9 | **Memoization Benchmark**<br>Create a component that runs a heavy computation (e.g., filtering/sorting an array of 10,000 items) on every render. Wrap it in `computed` or `useMemo`. Prove it only recalculates when the specific dependency changes, not on unrelated parent re-renders. | Heavy computation runs once. Subsequent renders (triggered by unrelated state) skip the computation entirely. |
| 10| **Skeleton Screen Implementation**<br>Replace a global "Loading..." spinner with a skeleton UI that matches the exact dimensions of the final content. Ensure it prevents Cumulative Layout Shift (CLS) while data fetches. | UI feels faster and more polished. Lighthouse reports no layout shift during the loading phase. |
