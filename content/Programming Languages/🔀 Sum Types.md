# theory
a sum type provides alternatives ("either/or" data).

```haskell
data Shape2D
  = Rect Double Double          -- width height
  | Circ Double                 -- radius
  | Poly [Vertex]               -- ≥3 vertices
```
### semantics
if $A$ has $|A|$ values, $B$ has $|B|$, then `Either A B` has $|A| + |B|$.
### exhaustive pattern matching
compiler checks **every constructor** is handled.
```haskell
area :: Shape2D -> Double
area (Rect w h) = w * h
area (Circ r)   = pi * r * r
area (Poly vs)  = polygonArea vs
```

use sum types for error handling:
```haskell
data Result a = Ok a | Err String
```

> [!note]
> consuming code must examine both cases, preventing silent failures.