# theory
a product type bundles multiple values together (“and” data).
### tuples
```haskell
type Point = (Double, Double)
```
### records
```haskell
data Circle = Circle
  { cx :: Double
  , cy :: Double
  , r  :: Double
  }
```

pattern‑match or use field selectors: `r circ`.
### semantics
if `A` has $|A|$ values and `B` has $|B|$ values, then $(A,B)$ has $|A|\times|B|$.
# implementation
constructors combine, patterns split:
```haskell
area :: Circle -> Double
area (Circle _ _ rad) = pi * rad * rad
```