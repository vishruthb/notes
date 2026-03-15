# Theory
Haskell programs are basically just sets of equations.
### Function Equations
```haskell
-- multiple equations, chosen top‑to‑bottom
fact 0 = 1
fact n = n * fact (n-1)
```
### Guards
```haskell
signum x | x > 0  =  1
         | x == 0 =  0
         | x < 0  = -1
```
### Local Bindings
```haskell
let y = x + 1 in y * y        -- expression‑level

foo x = result
  where result = x * x        -- equation‑level
```
### Pattern Matching Rules
- **left‑linearity** – no repeated variables.
- patterns tried in order, first match wins.
# Implementation
```haskell
(\x -> e) y     ==  let x = y in e
x : xs          ==  (:) x xs
(x,y)           ==  (,) x y
```