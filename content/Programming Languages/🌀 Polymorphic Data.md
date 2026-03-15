# Theory  
Parametric polymorphism lets one data description work for any element type.

```haskell
data List a = Nil | Cons a (List a)
```

`List` is a type constructor; `a` is a type parameter.

**Benefits**
- Code reuse – functions like `map`, `length` work for every `a`.
- Type safety – operations preserve element type.
# Example
```haskell
data Tree a
  = Empty
  | Node a (Tree a) (Tree a)

singleton :: a -> Tree a
singleton x = Node x Empty Empty
```

Generation and pattern-matching mirror concrete types:
```haskell
count :: Tree a -> Int
count Empty        = 0
count (Node _ l r) = 1 + count l + count r
```