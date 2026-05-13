# theory
a recursive type contains values of itself—essential for trees, linked lists, asts.

```haskell
data Tree a
  = Empty
  | Node a (Tree a) (Tree a)
```
### induction principle
define functions by:
1. **base case** – value built with base constructor(s).
2. **inductive case** – assume function defined for sub‑structures, build for whole.
# implementation
example: height
```haskell
height :: Tree a -> Int
height Empty        = 0
height (Node _ l r) = 1 + max (height l) (height r)
```

recursion mirrors data shape, ensuring termination when structure is finite.