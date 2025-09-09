# Preserving and Resetting State

React associates each piece of state which it’s holding with the correct component by where that component sits in the render tree.

- Same component at the same position preserves state
- Different components at the same position reset state

Resetting state at the same position with a `key`