# Level 07 — Professional

**Goal:** the work that separates a competent React developer from one other
engineers ask for advice. This level has no completion date.

---

## T1 — A real component library

**Build:** `Button`, `Input`, `Select`, `Dialog`, `Table` with:
- variants and sizes driven by props, no copy-pasted styles
- design tokens in CSS custom properties, not hardcoded hex
- full keyboard support and focus management (the Dialog is the hard one —
  focus trap, restore focus on close, Escape to dismiss)
- documented props

**Acceptance:** your Dialog passes keyboard testing without a library. Then
read Radix UI's dialog source and count what you missed. You will miss things;
that's the point of the exercise.

---

## T2 — Performance budget

**Build:** set a budget — LCP < 2.5s, INP < 200ms, CLS < 0.1, main bundle
< 150kB gzipped. Measure with Lighthouse. Fix what fails.

**Techniques:** route-level code splitting, image sizing and lazy loading,
font loading strategy, dependency audit (`npx vite-bundle-visualizer`).

**Acceptance:** before/after Lighthouse reports committed to the repo.

---

## T3 — Server rendering

**Build:** port one route to Next.js App Router. Server Components for the
data-fetching shell, Client Components only where interactivity requires it.

**Acceptance:** you can explain what "use client" actually does to the bundle,
and why fetching in a Server Component removes a waterfall you had in Vite.

---

## T4 — Read real source code

**Read, in this order** — small to large:
1. `zustand` — a few hundred lines, and you'll understand global state properly
2. `@tanstack/react-virtual` — the windowing math
3. React's own `useState` reconciler path — hard, skim it, come back later

**Acceptance:** write a short note explaining one technique you'd never have
invented yourself. Reading source is the fastest way from intermediate to
senior and almost nobody does it.

---

## T5 — Final exam: rebuild the dashboard

Now open `D:\React Js`.

**Build it from scratch, in this folder, without copying.** You may read the
old code to understand a decision, then close it and write your own.

The spec:
- DummyJSON products, one bootstrap request, client-side filter/sort/paginate
- KPI figures, two charts, a sortable paginated table
- Every chart has a table-view twin
- Loading, empty, error, and refetching states throughout
- Keyboard accessible, colour never the only signal
- The BMW M design system in `D:\React Js\DESIGN.md`
- Tests on the pure selectors
- No horizontal overflow at 390px — verify it, don't assume

**Acceptance:** you can justify every architectural decision out loud:
- why sort and page live inside the table but filters live in the page
- why the page resets during render instead of in an effect
- why the effect only writes state from fetch callbacks
- why `[...products].sort()` needs a `useMemo`
- why the status colours ship with icons and labels

If you can answer all five unprompted, you have arrived. That's the whole
point of this path.

---

## T6 — Ship something public

Deploy it. Write the README. Put it on GitHub. Then either publish a small npm
package, contribute a PR to a library you use, or write up one thing you
learned properly.

**Acceptance:** a stranger can use what you made without asking you questions.

---

## After this

Depth options, pick by interest, not by hype: animation (Motion), data viz
(D3 + React), 3D (React Three Fiber), real-time collaboration (CRDTs),
React Native, or compiler/tooling internals.

The thing that keeps compounding: keep reading source, keep measuring instead
of guessing, and keep writing the code yourself before asking anyone —
including me.
