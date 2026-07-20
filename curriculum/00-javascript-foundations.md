# Level 00 — JavaScript you must own first

**Why this level exists:** most "React bugs" are JavaScript bugs wearing a
costume. State that won't update is usually a mutation. A broken `useMemo` is
usually a misunderstanding of reference equality. Skipping this level is the
single most common reason people plateau at intermediate React.

Work in `src/00-js/`. These run in Node, not the browser: `node src/00-js/t1.js`.

---

## T1 — Array methods, from scratch

**Build:** implement your own versions without using the built-ins:

```js
myMap(array, fn)
myFilter(array, fn)
myReduce(array, fn, initial)
myGroupBy(array, keyFn)   // returns an object of arrays
```

Then rewrite each using the real built-in and compare.

**Acceptance:** `myGroupBy(products, p => p.category)` returns
`{ beauty: [...], laptops: [...] }`. Your `myReduce` works with and without an
initial value.

**Trap:** if you reach for a `for` loop inside `myMap`, that's correct. If you
reach for `.map()` inside `myMap`, you've missed the point.

---

## T2 — Reference vs value

**Build:** a file of ten predictions. Write your answer as a comment *first*,
then run and correct it.

```js
const a = { n: 1 };
const b = a;
b.n = 2;
console.log(a.n);           // predict:

const x = [1, 2];
const y = [1, 2];
console.log(x === y);       // predict:

const arr = [{ id: 1 }];
const copy = [...arr];
copy[0].id = 99;
console.log(arr[0].id);     // predict:
```

Add seven more of your own, at least three involving nested objects.

**Acceptance:** you can explain in one sentence why the spread copy did not
protect the nested object. Write that sentence in `PROGRESS.md`.

**This is the most important task in Level 00.** React's entire change
detection rests on it.

---

## T3 — Immutable updates

**Build:** given `const state = { user: { name: 'Ali', tags: ['a','b'] }, items: [...] }`,
write pure functions that return a **new** state:

- `addItem(state, item)`
- `removeItem(state, id)`
- `updateItem(state, id, patch)`
- `renameUser(state, name)`
- `addTag(state, tag)` — nested one level deeper

**Constraint:** no `push`, `splice`, `sort` on the original, no direct
assignment. Verify with `Object.freeze(state)` at the top — mutations will
throw in strict mode.

**Acceptance:** all five pass with the state frozen.

---

## T4 — Closures

**Build:**
1. `makeCounter()` returning `{ increment, get }` sharing private state.
2. The classic loop trap: reproduce it with `var`, fix it with `let`, then fix
   it a second way with an IIFE. Explain both fixes.
3. `once(fn)` — a wrapper that only ever calls `fn` the first time.

**Acceptance:** you can explain why `once` needs a closure and couldn't work
with a plain module-level variable if you had two independent wrapped
functions.

---

## T5 — Async and promises

**Build:**
1. Fetch `https://dummyjson.com/products?limit=5` with `.then()` chaining.
2. Rewrite with `async/await`.
3. Add error handling for a bad URL and a non-200 response — these are
   *different failures*, handle both.
4. `Promise.all` for three product IDs at once; then `Promise.allSettled` and
   explain the difference.
5. Write a deliberate race: fire two fetches where the slow one resolves last,
   and log which "wins". This is the bug you will fix properly in Level 03.

**Acceptance:** you can state why `fetch` does **not** reject on a 404.

---

## T6 — Modules

**Build:** split T1–T5 into modules with named exports, one barrel `index.js`,
and a single `main.js` that imports and runs them.

**Acceptance:** you can explain when you'd use a default export versus named,
and why most style guides now prefer named.

---

## Rebuild drill

Delete `t3.js` (immutable updates). Rebuild from memory. `git diff`.

---

## Self-quiz — answer without looking

1. Why does `[1,2] === [1,2]` return false?
2. What does spread copy, and what does it share?
3. Why does `fetch` not throw on a 500?
4. What's the difference between `Promise.all` and `Promise.allSettled`?
5. What is a closure, in one sentence, without the word "closure"?

If any answer is shaky, stay here. Level 01 will not fix it.
