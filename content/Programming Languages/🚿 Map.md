# theory
`map` captures the element‑wise transformation pattern.

**type:** $\text{map} :: (a \to b) \;\to\; [a] \;\to\; [b]$
### semantics
```haskell
map _ []     = []
map f (x:xs) = f x : map f xs
```
# example
```haskell
squares = map (^2)
shout   = map toUpper
```

eta‑contraction often yields point‑free style: `shout = map toUpper`. lazy and fusion‑friendly `map f (map g xs)` rewrites to `map (f . g) xs`.