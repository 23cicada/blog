## React — Best Practices

- Lift state up.
- Use `onSomething` for event prop names and `handleSomething` for handler function names.
- Don't mirror props in state.

### Why is mutating state not recommended in React?
When you store objects in state, mutating them will not trigger renders and will change the state in previous render “snapshots”.

### State as a snapshot
- Props, event handlers, and local variables are calculated from the state of that render.
- Setting state only changes it for the next render.
- A state variable's value never changes within a single render.
- React keeps the state values “fixed” within one render's event handlers.

See: https://react.dev/learn/state-as-a-snapshot

### Structuring state
1. Group related state.
2. Avoid contradictions in state.
3. Avoid redundant state.
4. Avoid duplication in state.
5. Avoid deeply nested state (keep it “flat”).

See: https://react.dev/learn/choosing-the-state-structure

### Batching
- React processes state updates after event handlers have finished running.


# Tips

- `useEffect()`: Effects let you run some code after rendering so that you can synchronize your component with some system outside of React (third-party API, network, etc).
