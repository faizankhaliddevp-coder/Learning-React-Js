# Level 05 — Real application architecture

**Goal:** the things every production React app has that a tutorial app never
does — routing, cached server state, real forms, and failure containment.

Work in `src/05-app/`. This level builds one app across all tasks.

---

## T1 — Routing

**Install:** `npm i react-router-dom`

**Build:** three routes — `/products`, `/products/:id`, `/cart`. A shared
layout with navigation. A 404 route.

**Then the part that matters:** move your filter state into the **URL** as
search params (`/products?q=phone&category=laptops`).

**Acceptance:** you can copy the URL, paste it in a new tab, and land on the
identical filtered view. Back/forward buttons move through filter history.
This is what makes a dashboard shareable and is skipped by most tutorials.

---

## T2 — Server state

**Install:** `npm i @tanstack/react-query`

**Build:** replace your `useFetch` from Level 03 with `useQuery`. Add:
- caching between route navigations,
- `staleTime` tuned deliberately (and be able to say why you chose it),
- a mutation that adds a product, with cache invalidation,
- optimistic update on the mutation, with rollback on error.

**Acceptance:** write a short note in `PROGRESS.md`: *what did TanStack Query
delete from my code, and what did it add?* You built the manual version in
Level 03 specifically so this comparison would mean something.

**Trap:** don't copy server data into `useState`. That's two sources of truth
and it is the single most common misuse of this library.

---

## T3 — Forms at scale

**Install:** `npm i react-hook-form zod @hookform/resolvers`

**Build:** a product create/edit form with a Zod schema — required title,
price positive, category from an enum, optional description with max length.
Show per-field errors. Disable submit while pending.

**Acceptance:** compare against your hand-rolled Level 01 T5 form and list
three things that got better and one that got worse (there is always one —
usually bundle size or indirection).

---

## T4 — Error boundaries and Suspense

**Build:**
1. An error boundary component. Throw deliberately in a child and confirm the
   rest of the app survives.
2. A route-level boundary with a retry action.
3. `React.lazy` + `Suspense` to code-split the detail route.

**Acceptance:** you can state what error boundaries do **not** catch — event
handlers, async code, and errors during SSR. This catches people out
constantly.

---

## T5 — Build: three-route product app

Pull it together:
- `/products` — URL-driven filters, cached list, virtualized if long
- `/products/:id` — detail, code-split, own loading and error states
- `/cart` — add/remove with optimistic updates

**Acceptance:**
- Refreshing any URL restores exact state.
- Every route has loading, empty, and error states.
- Throwing in one route doesn't white-screen the app.
- `npm run build` produces separate chunks per route — check the output.

---

## Rebuild drill

Delete the routing setup and rebuild from memory, URL params included.

---

## Self-quiz

1. Why is URL state better than component state for filters?
2. What is "server state" and why is it different from UI state?
3. What does `staleTime` control?
4. What do error boundaries not catch?
5. What does `React.lazy` do to your bundle?
