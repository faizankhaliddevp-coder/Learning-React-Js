# Level 01 — React fundamentals

**Goal:** build UI from components, state, and events without copying patterns
you don't understand.

Work in `src/01-fundamentals/`. Render one task at a time from `App.jsx`.

**Data for this level:** hardcode a `products.js` array of ~12 objects with
`id, title, category, price, rating, stock`. No fetching yet — fetching is
Level 03, and mixing it in now will hide which part broke.

---

## T1 — Components and props

**Build:** `<ProductCard product={...} />` rendering title, category, price,
and rating. Then `<ProductGrid products={...} />` that maps over the array.

**Constraints:** `ProductCard` takes exactly one prop. No state anywhere yet.

**Acceptance:** adding a product to the array changes the UI with no other
edit. `ProductCard` has no idea an array exists.

**Trap:** if you find yourself passing `index` into `ProductCard`, ask why it
needs to know its own position. It doesn't.

---

## T2 — State and events

**Build, in order:**
1. A counter with increment, decrement, reset.
2. A show/hide toggle for product details.
3. A controlled text input that echoes its value below.

**Then a deliberate experiment:** write

```jsx
setCount(count + 1);
setCount(count + 1);
```

Predict the result. Run it. Then replace with the updater form
`setCount(c => c + 1)` twice and predict again.

**Acceptance:** you can explain why the first version increments by one, using
the words "this render's value".

---

## T3 — Lists and keys

**Build:** render the products with `key={index}`. Add a "shuffle" button and
a text input *inside* each card. Type into the third card's input, then
shuffle.

**Observe the bug.** Write down exactly what happened before fixing it.

Then switch to `key={product.id}` and repeat.

**Acceptance:** you can explain what React did with the input's state and why
identity-based keys fixed it. This is the clearest demonstration of
reconciliation you will get — do not skip the broken version.

---

## T4 — Conditional rendering and the four states

**Build:** a `status` variable you set by hand (`'loading' | 'error' | 'empty'
| 'success'`) and render a different UI for each. Add buttons to switch.

**Constraints:** no nested ternaries more than one level deep. If it gets
ugly, that's a signal to use early returns or a lookup object.

**Acceptance:** all four states look deliberate, not like placeholders. Most
developers ship only `success` — building the other three by hand now is what
stops that habit forming.

---

## T5 — Controlled forms

**Build:** an "add product" form with title, price, category (select), and
in-stock (checkbox). On submit, add to the list. Validate: title required,
price must be a positive number. Show errors below each field.

**Constraints:** no form library. Labels attached to every input with
`htmlFor`. No placeholder-as-label.

**Acceptance:** submitting with an empty title shows an error and does not add
a row. Keyboard-only submission works.

---

## T6 — Build: product list with search

**Build:** combine everything. A page with a search box filtering the list by
title, plus the add-form from T5.

**Constraint that matters:** the filtered list must **not** be stored in
state. Compute it during render. If you wrote `const [filtered, setFiltered]
= useState([])`, delete it and try again — that's the Level 02 lesson arriving
early, and it's better to feel it than be told it.

**Acceptance:** one `useState` for the query, one for the products. No third.

---

## Rebuild drill

Delete `ProductGrid.jsx` and the search page. Rebuild from memory. `git diff`.

---

## Self-quiz

1. What does `key` actually do? What breaks without a stable one?
2. Why does calling `setState` twice with the same value only increment once?
3. What makes an input "controlled"?
4. Why should the filtered array not be state?
5. When does a component re-render?
