# Theory  
Folds reduce a list to a single value by combining elements with a binary operator.

| name    | type                                      | accumulation order                |
| ------- | ----------------------------------------- | --------------------------------- |
| `foldr` | $$ (a \to b \to b) \to b \to [a] \to b $$ | right‑associative                 |
| `foldl` | $$ (b \to a \to b) \to b \to [a] \to b $$ | left‑associative / tail‑recursive |

```haskell
foldr op z []     = z
foldr op z (x:xs) = x `op` foldr op z xs

foldl op z []     = z
foldl op z (x:xs) = foldl op (z `op` x) xs
```
### Use Cases
- `foldr` works on **infinite lists** (lazy, needs only what `op` demands).
- `foldl'` (strict version) is memory‑friendly for large finite lists.
$$\text{foldr }(+)0[1,2,3]⟹1+(2+(3+0))$$
$$\text{foldl }(+)0[1,2,3]⟹((0+1)+2)+3$$
# Implementation
```haskell
sumR  = foldr (+) 0
sumL  = foldl' (+) 0

reverse = foldl (flip (:)) []
```