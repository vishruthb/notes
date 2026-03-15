# Theory  
A higher‑order function (HOF) takes functions as arguments or returns them. Allows us to capture common traversal/computation patterns once and reuse forever.

| HOF                         | Pattern                                             |
| --------------------------- | --------------------------------------------------- |
| [[🚿 Map]]                  | apply a transformation to each element              |
| [[🔍 Filter]]               | keep elements satisfying a predicate                |
| [[📐 Foldr & Foldl]]        | reduce a list using an operator                     |
| [[➡️ Function Combinators]] | glue functions together (`.`) or swap args (`flip`) |

### Example – Refactoring with `map`
```haskell
shout   = map toUpper
squares = map (^2)
```

No explicit recursion needed; clarity & composability improve.
# Implementation
```haskell
map :: (a -> b) -> [a] -> [b]
map f []     = []
map f (x:xs) = f x : map f xs
```
Compiler optimizations like _fusion_ rewrite chains (`map f . map g`) into one traversal.