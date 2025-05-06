# Theory
A sum type provides alternatives ("either/or" data).

```haskell
data Shape2D
  = Rect Double Double          -- width height
  | Circ Double                 -- radius
  | Poly [Vertex]               -- ≥3 vertices
```
### Semantics
If $A$ has $|A|$ values, $B$ has $|B|$, then `Either A B` has $|A| + |B|$.
### Exhaustive Pattern Matching
Compiler checks **every constructor** is handled.
```haskell
area :: Shape2D -> Double
area (Rect w h) = w * h
area (Circ r)   = pi * r * r
area (Poly vs)  = polygonArea vs
```

Use sum types for error handling:
```haskell
data Result a = Ok a | Err String
```

> [!NOTE]
> Consuming code must examine both cases, preventing silent failures.