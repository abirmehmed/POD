# Modern Frontend Architecture: Set 1
## TypeScript Fundamentals, Component Architecture & State Management

## 📘 10 Conceptual Questions
1. **TypeScript vs. JavaScript at Scale:** Why is "just using JSDoc" or relying on runtime checks insufficient for large frontend codebases? Explain how strict TypeScript prevents entire categories of bugs (undefined errors, prop mismatches) before the code even runs.
2. **Component Composition vs. Inheritance:** Why is inheritance (e.g., extending a base component class) considered an anti-pattern in modern frontend? How do Slots (Vue) or Render Props/Children (React) enable better, more flexible composition?
3. **The Three Types of State:** What is the difference between **Local State** (UI toggles), **Global State** (auth user, shopping cart), and **Server State** (fetched database records)? Why is using a global store (like Redux/Pinia) for Server State an anti-pattern?
4. **Prop Drilling vs. Context/Provide-Inject:** What is prop drilling, and why does it make refactoring difficult? When should you use Context API (React) or Provide/Inject (Vue) vs. passing props vs. using a state manager?
5. **Immutability in Frontend State:** Why is mutating state directly (e.g., `cart.items.push(newItem)`) dangerous in frameworks like React or Vue 3? How does immutability enable efficient change detection and time-travel debugging?
 is the difference between a "dumb" (presentational) component and a "smart" (container) component? Why is separating UI rendering from data-fetching logic critical for testability?
7. **Composables / Custom Hooks:** What is the purpose of extracting logic into a `useSomething` function? How does this pattern solve the problem of "God Components" and enable logic reuse without UI coupling?
8. **Controlled vs. Uncontrolled Components:** In form handling, what is the difference between letting the DOM manage the input value (uncontrolled) vs. the framework state managing it (controlled)? When is each appropriate?
9. **Full-Stack Type Safety:** How do you ensure the frontend TypeScript types perfectly match the Laravel backend API responses? Explain approaches like manual typing, OpenAPI code generation, or tools like Ziggy/Zod.
10. **CSS Architecture in Modern Frontend:** Compare Utility-First (Tailwind), CSS Modules, and CSS-in-JS. Why has Utility-First become the dominant paradigm for component-based architectures, and what are its limitations?

---

## 🛠️ 10 Practical Tasks

| # | Task | Success Criteria |
|---|------|------------------|
| 1 | **Strict TypeScript Migration**<br>Take an existing JavaScript component (e.g., a Product Card). Migrate it to strict TypeScript. Define explicit `Props` and `Emits` (or return types) interfaces. Enable `strict: true` in `tsconfig.json`. | Zero `any` types. IDE provides perfect autocomplete for props. Build fails if a required prop is missing or typed incorrectly. |
| 2 | **Build a Headless UI Component**<br>Create a reusable `Modal` or `Dropdown` component that handles *only* the logic (open/close, focus trapping, ESC key) but renders *no* default HTML. Use Slots/Children to let the parent define the UI. | Component is 100% style-agnostic. Parent component fully controls the visual appearance. Logic is thoroughly tested and reusable. |
| 3 | **Extract Logic into a Composable/Hook**<br>Find a component with >50 lines of reactive state and event handlers (e.g., a search bar with debouncing). Extract this logic into a `useSearch` composable/hook. | Component shrinks to <15 lines. `useSearch` can be imported and used in any other component. Logic is decoupled from the DOM. |
| 4 | **Implement Global State (Pinia/Zustand)**<br>Create a global store for the Shopping Cart. Implement actions for `addItem`, `removeItem`, and `clearCart`. Ensure the store is fully typed and persists to `localStorage` on change. | Cart state is accessible from any component. Actions mutate state immutably. Refreshing the page restores the cart from storage. |
| 5 | **Server State Management (TanStack Query / Vue Query)**<br>Replace a manual `onMounted`/`useEffect` fetch with a dedicated data-fetching library. Implement caching, automatic retries, and a loading/error state. | Data is cached across route navigations. Stale data is automatically refetched in the background. No manual `isLoading` boolean juggling. |
| 6 | **Full-Stack Type Sharing**<br>Define a `Product` interface in TypeScript. Use a tool (like `zod` schema sharing, or a simple shared `types.ts` file if using Inertia) to ensure the Laravel API response matches this type exactly. | Changing the backend requires updating the shared type, which immediately flags frontend errors. No runtime `undefined` property access. |
| 7 | **Refactor a "God Component"**<br>Take a monolithic component (e.g., a full Checkout Page). Split it into: `CheckoutForm` (smart), `OrderSummary` (dumb), `PaymentMethodSelector` (dumb), and `useCheckout` (logic). | Each file has a single responsibility. Dumb components have zero business logic. Smart component orchestrates them cleanly. |
| 8 | **Type-Safe Form Handling**<br>Build a registration form using a library like `React Hook Form` or `Vee-Validate`, paired with `Zod` or `Yup` for schema validation. Ensure the form values type matches the schema type. | Form prevents submission on invalid data. Error messages are strongly typed. No manual `v-if` or `if (error)` checks scattered in the template. |
| 9 | **Dark Mode without Layout Shift**<br>Implement a dark mode toggle. Use CSS variables for colors and a script in the `<head>` to read `localStorage` *before* the DOM paints, preventing the "flash of unstyled content" (FOUC). | Toggle works instantly. Refreshing the page maintains the theme with zero visual flicker. Tailwind `dark:` classes apply correctly. |
| 10| **Frontend Quality Gates**<br>Configure `eslint-plugin-vue` or `eslint-plugin-react`, `prettier`, and `typescript-eslint`. Add a pre-commit hook (Husky) or CI step that blocks commits with linting errors or `console.log` statements. | Running `npm run lint` catches all issues. Bad commits are blocked automatically. Codebase maintains a consistent style. |
