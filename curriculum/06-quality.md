# Level 06 — Quality: tests, types, accessibility, CI

**Goal:** the difference between "it works on my machine" and "someone else can
change it safely." This is the level that gets you hired and keeps you hired.

Work in `src/05-app/` — you are adding quality to the app you already built.

---

## T1 — Unit tests on pure functions

**Install:** `npm i -D vitest`

**Build:** tests for the pure filter/sort functions you extracted in Level 02
T5. Start here because pure functions need no mocking, no DOM, no setup.

**Cover deliberately:**
- empty input array
- no matches
- boundary values (if "low stock" is `< 25`, test 24, 25, and 26)
- case-insensitive search
- sort stability

**Acceptance:** `npm test` passes. Then **break the implementation on purpose**
and confirm a test fails. A test suite that passes against broken code is
worse than none — it's false confidence.

---

## T2 — Component tests

**Install:** `npm i -D @testing-library/react @testing-library/user-event jsdom`

**Build:** tests for the search page:
- renders the empty state when no products match
- typing filters the list
- clicking a product navigates to detail

**Constraint:** query by **role and accessible name**
(`getByRole('button', { name: /add/i })`), never by class name or test id
unless there's no alternative.

**Acceptance:** you can explain why role-based queries are better — they break
when the UI becomes inaccessible, which is exactly when you want to know.

---

## T3 — Hook tests

**Build:** tests for your custom hook using `renderHook`. Mock `fetch` with
`vi.fn()`. Cover loading → success, loading → error, and abort on unmount.

**Acceptance:** the abort test actually asserts the abort happened, not just
that nothing crashed.

---

## T4 — TypeScript migration

**Build:** convert incrementally, in this order:
1. `tsconfig.json`, allow JS alongside TS
2. pure functions first — highest value per line
3. the custom hooks
4. components last

**Constraints:** `strict: true`. No `any`. Where you're tempted by `any`, use
`unknown` and narrow it.

**Acceptance:** a type error the compiler caught that you had not noticed.
Write it in `PROGRESS.md` — that's your evidence TypeScript earned its cost.

---

## T5 — Accessibility audit

**Build:** audit and fix:
- keyboard-only: reach and operate every control, visible focus throughout
- every input has a real `<label>`
- sortable table headers use `aria-sort`
- async results announced via `aria-live`
- colour is never the only signal — pair it with an icon or text
- run axe DevTools and fix what it finds

**Acceptance:** you complete a full add-product flow using only the keyboard,
and you can name one thing axe flagged that you'd never have spotted.

---

## T6 — CI

**Build:** a GitHub Actions workflow running lint, typecheck, test, and build
on every push. Make it fail once on purpose, then fix it.

**Acceptance:** a green check on a pull request in your own repo.

---

## Self-quiz

1. What makes a test valuable rather than just present?
2. Why query by role instead of test id?
3. What does `strict: true` actually turn on?
4. How do you announce async changes to a screen reader?
5. What should CI run, and why in that order?
