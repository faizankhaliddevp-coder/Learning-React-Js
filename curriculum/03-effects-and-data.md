# Level 03 — Effects and the outside world

**Goal:** understand that effects are an escape hatch for synchronizing with
things outside React — not a lifecycle hook, not a place to compute state.

Most React code you'll see in the wild misuses effects. After this level you
should be able to spot it.

Work in `src/03-effects/`. API: `https://dummyjson.com/docs`.

---

## T1 — Effects and cleanup

**Build three effects, each with correct cleanup:**
1. A `setInterval` clock. Unmount it with a toggle button.
2. A `window` resize listener showing the width.
3. A `document.title` sync from a state value.

**Then break each one on purpose:** remove the cleanup, unmount, and observe.
The interval keeps firing on a dead component. Write what you saw.

**Acceptance:** you can state which of the three genuinely needs cleanup and
why the third one differs.

---

## T2 — StrictMode double-invoke

**Build:** an effect that `console.log`s on mount. Run it. It logs twice.

**Do not "fix" this by removing StrictMode.** Instead, answer: why does React
do this deliberately in development?

**Acceptance:** you can explain that it simulates a mount → unmount → remount
to surface missing cleanup, and that an effect which breaks under it is
already broken in production — you just wouldn't have found out yet.

---

## T3 — Fetch with real states

**Build:** load `/products?limit=20` and render all four states from Level 01
T4 — but real this time.

**Constraints:**
- No library. `fetch` only.
- Handle a non-200 response as an error (remember Level 00 T5).
- Test the error path by pointing at a bad URL.
- Test loading by throttling to Slow 3G in DevTools Network tab.

**Acceptance:** all four states are reachable and you have *seen* each one.

---

## T4 — The race condition

**Build:** a search box that fetches `/products/search?q=` on every keystroke.

Type "phone" quickly. With DevTools throttling on, you will eventually see
results for `pho` overwrite results for `phone`.

**Reproduce it first. Screenshot it. Then fix it two ways:**
1. An `ignore` boolean flag in the cleanup.
2. An `AbortController`.

**Acceptance:** you can explain what each fix does differently — one discards
a response that already arrived, the other cancels the request in flight — and
which you'd choose when the request is expensive on the server.

**This task is the single most valuable one in Level 03.** Most developers have
never knowingly fixed a race condition.

---

## T5 — Extract a custom hook

**Build:** `useFetch(url)` returning `{ data, status, error, reload }`, with
the abort logic from T4 inside it.

**Constraints:** the hook file exports only the hook — no components. (If you
export both, Fast Refresh breaks and ESLint will tell you; when it does, read
the message properly instead of silencing it.)

**Acceptance:** two different components use it against two different URLs
without modification.

---

## T6 — Build: debounced live search

**Build:** combine T4 and T5 into a search page that:
- debounces input by 300ms so you don't fetch on every keystroke,
- aborts superseded requests,
- keeps the previous results visible while loading instead of flashing a
  skeleton,
- shows a proper empty state for "no matches".

**Acceptance:** typing fast produces exactly one request after you stop.
Verify in the Network tab — don't assume.

---

## Rebuild drill

Delete `useFetch.js`. Rebuild from memory, including abort handling.
`git diff`. This one is hard; expect to miss something.

---

## Self-quiz

1. When is `useEffect` the right tool? Give two genuine cases.
2. What does the dependency array actually do?
3. Why does an effect run twice in development?
4. What is a race condition here, and what are two fixes?
5. Why is `setState` at the top of an effect body a smell?
