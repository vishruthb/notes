# theory
a higher‑order function (hof) takes functions as arguments or returns them. allows us to capture common traversal/computation patterns once and reuse forever.

| hof                         | pattern                                             |
| --------------------------- | --------------------------------------------------- |
| [[🚿 Map|🚿 map]]                  | apply a transformation to each element              |
| [[🔍 Filter|🔍 filter]]               | keep elements satisfying a predicate                |
| [[📐 Foldr & Foldl|📐 foldr & foldl]]        | reduce a list using an operator                     |
| [[➡️ Function Combinators|➡️ function combinators]] | glue functions together (`.`) or swap args (`flip`) |

### example – refactoring with `map`
```haskell
shout   = map toUpper
squares = map (^2)
```

no explicit recursion needed; clarity & composability improve.
# implementation
```haskell
map :: (a -> b) -> [a] -> [b]
map f []     = []
map f (x:xs) = f x : map f xs
```
compiler optimizations like _fusion_ rewrite chains (`map f . map g`) into one traversal.