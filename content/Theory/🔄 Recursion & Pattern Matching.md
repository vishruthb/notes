# Theory
Recursion and pattern matching is Haskell’s control structure.
### Structural Recursion
Break a value into constructors, solve sub‑parts, re‑combine.
```haskell
length [] = 0

length (_:xs) = 1 + length xs
```
### Mutual Recursion
```haskell
even', odd' :: Int -> Bool

even' 0 = True

even' n = odd' (n-1)

odd' 0 = False

odd' n = even' (n-1)
```
### Tail Recursion
Carry accumulator so last action is the recursive call.
```haskell
sumTR xs = go 0 xs
  where
    go acc []     = acc
    go acc (y:ys) = go (acc + y) ys
```
# Implementation
```haskell
-- literals
0    -> base case

-- constructor patterns
(Node x l r)  -- match tree
(_:xs)        -- match non‑empty list

-- wildcard
_             -- ignore value
```