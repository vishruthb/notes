# theory
algebraic data types (adts) build complex data from sums and products.
- [[📦 Product Types|📦 product types]] combine fields (logical **and**).
- [[🔀 Sum Types|🔀 sum types]] choose between constructors (logical **or**).
- [[🌳 Recursive Types|🌳 recursive types]] reference themselves.
- [[🌀 Polymorphic Data|🌀 polymorphic data]] abstracts over element types.
### expressiveness
tuples, lists, `Maybe`, `Either`, user trees—all adts.
### example
```haskell
data Result a
  = Success a
  | Failure String
```

> [!note]
> pattern matching + compiler exhaustiveness = strong invariants.
# implementation
define once, then use in pattern matches, derive instances:
```haskell
data Color = Red | Green | Blue
  deriving (Eq, Show)
```

deriving saves boilerplate for `Eq`, `Ord`, `Read`, `Show`, etc.