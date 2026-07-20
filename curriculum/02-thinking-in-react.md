# Level 02 — Thinking in React

**Goal:** stop asking "which hook do I need?" and start asking "where does this
state belong, and does it need to exist at all?"

This is the level that separates people who can follow tutorials from people
who can design a component tree. It has the least new API and the most value.

Work in `src/02-thinking/`.

---

## T1 — Delete redundant state

**Start from this deliberately bad component** (type it out, don't paste from
memory of a better version):

```jsx
function Cart({ items }) {
  const [total, setTotal] = useState(0);
  const [count, setCount] = useState(0);
  const [isEmpty, setIsEmpty] = useState(true);

  useEffect(() => {
    setTotal(items.reduce((s, i) => s + i.price * i.qty, 0));
    setCount(items.length);
    setIsEmpty(items.length === 0);
  }, [items]);

  // ...
}
```

**Build:** the same component with **zero** `useState` and **zero**
`useEffect`.

**Acceptance:** you can name three concrete bugs the original has. (Hints to
check yourself against: what renders on the very first paint? what happens if
`items` changes twice quickly? what if a parent passes a new array identity
every render?)

**The rule you are learning:** if you can compute it during render from props
or existing state, it is not state.

---

## T2 — Lift state, but only as far as needed

**Build:** a page with a `<CategoryFilter>` and a `<ProductGrid>` as siblings.
The filter must drive the grid.

Then add a `<SortControl>` that only the grid uses.

**Acceptance:** the filter value lives in the shared parent. The sort value
lives... you decide, and write a one-line comment justifying it. Getting this
wrong is the most common cause of unnecessary re-renders in real apps.

**Trap:** "lift everything to the top so it's available" produces a component
that re-renders the world on every keystroke. State goes at the *lowest* common
ancestor, not the highest.

---

## T3 — Composition instead of prop drilling

**Build:** a `<Panel>` that renders a header, a body, and an optional footer.
Version A passes `title`, `bodyText`, `footerButtons` as props. Version B uses
`children` and slot props:

```jsx
<Panel>
  <Panel.Header>Inventory</Panel.Header>
  <Panel.Body><ProductGrid /></Panel.Body>
</Panel>
```

**Acceptance:** you can state one thing version B does easily that version A
cannot, and one thing version A does better. Both answers exist — this is a
trade-off, not a rule.

---

## T4 — Component boundaries

**Build:** take the T6 page from Level 01 and split it into components. Then
write a comment above each one answering: *why is this a separate component?*

Valid reasons: it's reused, it isolates re-renders, it has its own state, it
makes the parent readable. "It was getting long" is the weakest one — say so
if that's the real reason.

**Acceptance:** no component over ~80 lines. Every split has a stated reason.
At least one component you *considered* extracting and deliberately did not,
with a note explaining why.

---

## T5 — Build: multi-filter product browser

**Build:** search text + category select + sort dropdown + a result count,
all driving one list.

**Constraints:**
- Zero derived state. The filtered, sorted array is computed during render.
- Filter state lives in the page; sort state lives wherever T2 taught you.
- Extract the filtering and sorting into **pure functions in a separate file**
  that take `(products, options)` and return an array. They must not import
  React.

**Acceptance:** you can call `filterProducts` from a plain Node script and it
works. That separation is what makes logic testable in Level 06 — you're
setting yourself up.

---

## Rebuild drill

Delete the pure-functions file. Rebuild from memory. `git diff`.

---

## Self-quiz

1. Where does a piece of state belong?
2. Give a case where `useEffect` to sync state is genuinely correct. (They
   exist, but they are rarer than you'd think — external systems only.)
3. What's the cost of lifting state too high?
4. Why keep filtering logic out of the component file?
