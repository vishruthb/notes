# theory
a list is an ordered, homogeneous sequence.
### constructors
```haskell
[]      -- empty
(:)     -- cons  (x : xs)
```

`[a]` is syntactic sugar for `[] | (:) a [a]`.
### ranges
```haskell
[1..5]      ==  [1,2,3,4,5]
[2,4..10]   ==  [2,4,6,8,10]
```
### comprehensions
```haskell
[(i,j) | i <- [1..3], j <- [1..i], gcd i j == 1]
```

semantics: _generate_ (`<-`), _filter_ (guards), _yield_ expression.
# implementation
the core library offers `null`, `head`, `tail`, `length`, `(++)`.

idiomatic pattern matching:
```haskell
len :: [a] -> Int
len []     = 0
len (_:xs) = 1 + len xs
```