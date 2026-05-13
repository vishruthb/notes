# theory
recursion and pattern matching is haskell’s control structure.
### structural recursion
break a value into constructors, solve sub‑parts, re‑combine.
```haskell
length [] = 0

length (_:xs) = 1 + length xs
```
### mutual recursion
```haskell
even', odd' :: Int -> Bool

even' 0 = True

even' n = odd' (n-1)

odd' 0 = False

odd' n = even' (n-1)
```
### tail recursion
carry accumulator so last action is the recursive call.
```haskell
sumTR xs = go 0 xs
  where
    go acc []     = acc
    go acc (y:ys) = go (acc + y) ys
```
# implementation
```haskell
-- literals
0    -> base case

-- constructor patterns
(Node x l r)  -- match tree
(_:xs)        -- match non‑empty list

-- wildcard
_             -- ignore value
```