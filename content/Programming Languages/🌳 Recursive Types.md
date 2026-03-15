# Theory  
A recursive type contains values of itself—essential for trees, linked lists, ASTs.

```haskell
data Tree a
  = Empty
  | Node a (Tree a) (Tree a)
```
### Induction Principle
Define functions by:
1. **Base case** – value built with base constructor(s).
2. **Inductive case** – assume function defined for sub‑structures, build for whole.
# Implementation
Example: Height
```haskell
height :: Tree a -> Int
height Empty        = 0
height (Node _ l r) = 1 + max (height l) (height r)
```

Recursion mirrors data shape, ensuring termination when structure is finite.