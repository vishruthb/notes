# Theory  
Algebraic Data Types (ADTs) build complex data from sums and products.
- [[📦 Product Types]] combine fields (logical **and**).  
- [[🔀 Sum Types]] choose between constructors (logical **or**).  
- [[🌳 Recursive Types]] reference themselves.  
- [[🌀 Polymorphic Data]] abstracts over element types.
### Expressiveness  
Tuples, lists, `Maybe`, `Either`, user trees—all ADTs.
### Example  
```haskell
data Result a
  = Success a
  | Failure String
```

> [!Note]
> Pattern matching + compiler exhaustiveness = strong invariants.
# Implementation
Define once, then use in pattern matches, derive instances:
```haskell
data Color = Red | Green | Blue
  deriving (Eq, Show)
```

Deriving saves boilerplate for `Eq`, `Ord`, `Read`, `Show`, etc.