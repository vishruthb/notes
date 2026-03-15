# Theory  
`filter` bottles the **selection** pattern: traverse a list, keep elements that satisfy a **predicate** $p : a \to \text{Bool}$.

**Type**: $\text{filter} :: (a \to \text{Bool}) \;\to\; [a] \;\to\; [a]$
### Semantics 
```haskell
filter _  []     = []
filter p (x:xs)  = if p x
		           then x : filter p xs   -- keep
                   else     filter p xs  -- skip
```
# Examples
```haskell
isEven  x = x `mod` 2 == 0
evens   xs = filter isEven xs        -- [2,4]

isFour  w = length w == 4
fourChr xs = filter isFour xs        -- ["must","work"]
```

Straight recursion as shown above is idiomatic; under the hood `filter` is tail‑recursive and works in tandem with list comprehensions.

```haskell
evens = [ x | x <- xs, even x ]
```