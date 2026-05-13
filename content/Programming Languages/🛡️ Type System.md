# theory
haskell has a static, [hindley-milner](https://en.wikipedia.org/wiki/Hindley%E2%80%93Milner_type_system), parametric polymorphic type system.
### annotation
```haskell
haskellIsAwesome :: Bool
```
### type inference
compilers infer principal type, annotations are optional but recommended.
#### arrow types
```haskell
(Int -> Bool) -> [Int] -> [Bool]
```
#### polymorphism
```haskell
id :: a -> a
```

**type classes:** `Num`, `Eq`, `Show` give ad‑hoc polymorphism (overloading).
# implementation
check types in ghci:
```haskell
:t map
-- map :: (a -> b) -> [a] -> [b]
```