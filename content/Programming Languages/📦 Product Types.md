# Theory
A product type bundles multiple values together (“and” data).
### Tuples
```haskell
type Point = (Double, Double)
```
### Records
```haskell
data Circle = Circle
  { cx :: Double
  , cy :: Double
  , r  :: Double
  }
```

Pattern‑match or use field selectors: `r circ`.
### Semantics
If `A` has $|A|$ values and `B` has $|B|$ values, then $(A,B)$ has $|A|\times|B|$.
# Implementation
Constructors combine, patterns split:
```haskell
area :: Circle -> Double
area (Circle _ _ rad) = pi * rad * rad
```