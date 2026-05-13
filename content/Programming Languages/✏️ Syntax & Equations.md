# theory
haskell programs are basically just sets of equations.
### function equations
```haskell
-- multiple equations, chosen top‑to‑bottom
fact 0 = 1
fact n = n * fact (n-1)
```
### guards
```haskell
signum x | x > 0  =  1
         | x == 0 =  0
         | x < 0  = -1
```
### local bindings
```haskell
let y = x + 1 in y * y        -- expression‑level

foo x = result
  where result = x * x        -- equation‑level
```
### pattern matching rules
- **left‑linearity** – no repeated variables.
- patterns tried in order, first match wins.
# implementation
```haskell
(\x -> e) y     ==  let x = y in e
x : xs          ==  (:) x xs
(x,y)           ==  (,) x y
```