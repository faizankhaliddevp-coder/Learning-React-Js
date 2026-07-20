# Level 04 — Performance and referential identity

**Goal:** optimize from measurement, never from superstition. Most `useMemo`
in real codebases is cargo cult and some of it is a net loss.

Install React DevTools in your browser before starting. You cannot do this
level without the Profiler.

Work in `src/04-performance/`.

---

## T1 — Identity, predicted

**Build:** a prediction file, same format as Level 00 T2.

```jsx
const Child = React.memo(function Child({ config, onPick }) {
  console.log('Child rendered');
  return <button onClick={onPick}>pick</button>;
});

function Parent() {
  const [n, setN] = useState(0);
  const config = { mode: 'a' };            // predict: does Child re-render?
  const onPick = () => console.log('x');   // predict:
  return <><button onClick={() => setN(n+1)}>{n}</button>
          <Child config={config} onPick={onPick} /></>;
}
```

Predict, run, then fix so `Child` genuinely stops re-rendering.

**Acceptance:** you can explain why `React.memo` alone did nothing, and name
the two things that had to change.

---

## T2 — Profile before optimizing

**Build:** render 2,000 product rows with a filter input. Open the Profiler,
record a keystroke, and read the flamegraph.

**Write down, before changing any code:** which component takes longest, and
how many components rendered.

**Acceptance:** you have real numbers in `PROGRESS.md`. Any optimization you
do from here must cite them.

---

## T3 — Memoize what you measured

**Build:** apply `useMemo` / `useCallback` / `React.memo` to exactly the
things T2 identified. Re-profile after each change and record the delta.

**Constraint:** if a change doesn't measurably help, **revert it**. Memo has a
cost — comparison work plus retained memory — and unmeasured memo is often
slower than none.

**Acceptance:** at least one optimization you tried, measured, and reverted.
If everything you tried helped, you weren't trying hard enough.

---

## T4 — useDeferredValue versus debounce

**Build:** the same filter three ways — plain, debounced 300ms, and with
`useDeferredValue`. Profile all three.

**Acceptance:** you can describe the felt difference. Debounce delays the work;
`useDeferredValue` starts it immediately at low priority and keeps the input
responsive while stale results stay on screen. They solve different problems
and are sometimes used together.

---

## T5 — Virtualization

**Build:** render 10,000 rows. Measure. Then add `@tanstack/react-virtual` and
measure again.

**Acceptance:** you can explain what's in the DOM before and after, and state
the accessibility cost virtualization introduces (hint: what happens to
Ctrl+F, and to a screen reader's sense of list length?).

---

## T6 — Build: 5,000-item search that stays smooth

Combine it all. Target: typing feels instant on a mid-range laptop with
CPU throttled 4×.

**Acceptance:** a before/after Profiler screenshot pair in your repo.

---

## Rebuild drill

Delete the virtualized list. Rebuild from memory.

---

## Self-quiz

1. Why doesn't `React.memo` help when you pass an inline object?
2. What does `useCallback` actually memoize?
3. When is `useMemo` a net loss?
4. Difference between debouncing and deferring?
5. What does virtualization cost you?
