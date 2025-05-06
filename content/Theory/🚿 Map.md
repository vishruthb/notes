# Theory
`map` captures the element‑wise transformation pattern.

**Type:** $\text{map} :: (a \to b) \;\to\; [a] \;\to\; [b]$
### Semantics  
```haskell
map _ []     = []
map f (x:xs) = f x : map f xs
```
# Example
```haskell
squares = map (^2)
shout   = map toUpper
```

Eta‑contraction often yields point‑free style: `shout = map toUpper`. Lazy and fusion‑friendly `map f (map g xs)` rewrites to `map (f . g) xs`.