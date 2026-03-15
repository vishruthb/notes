# Theory  
A list is an ordered, homogeneous sequence.
### Constructors  
```haskell
[]      -- empty
(:)     -- cons  (x : xs)
```

`[a]` is syntactic sugar for `[] | (:) a [a]`.
### Ranges
```haskell
[1..5]      ==  [1,2,3,4,5]
[2,4..10]   ==  [2,4,6,8,10]
```
### Comprehensions
```haskell
[(i,j) | i <- [1..3], j <- [1..i], gcd i j == 1]
```

Semantics: _generate_ (`<-`), _filter_ (guards), _yield_ expression.
# Implementation
The core library offers `null`, `head`, `tail`, `length`, `(++)`.

Idiomatic pattern matching:
```haskell
len :: [a] -> Int
len []     = 0
len (_:xs) = 1 + len xs
```