# Theory
Haskell has a static, [Hindley-Milner](https://en.wikipedia.org/wiki/Hindley%E2%80%93Milner_type_system), parametric polymorphic type system.
### Annotation
```haskell
haskellIsAwesome :: Bool
```
### Type Inference
Compilers infer principal type, annotations are optional but recommended.
#### Arrow Types
```haskell
(Int -> Bool) -> [Int] -> [Bool]
```
#### Polymorphism
```haskell
id :: a -> a
```

**Type Classes:** `Num`, `Eq`, `Show` give ad‑hoc polymorphism (overloading).
# Implementation
Check types in GHCi:
```haskell
:t map
-- map :: (a -> b) -> [a] -> [b]
```