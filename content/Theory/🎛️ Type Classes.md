# Theory
- **Purpose.** Type classes enable _ad-hoc polymorphism_: the same operator (e.g. `+`, `==`) can have different implementations for different types, unlike parametric polymorphism where one implementation works for all types.
- **Constrained types.** A function’s type can be _qualified_ with a class constraint, e.g.
```haskell
    (+) :: Num a => a -> a -> a
```

Read “for any `a` that is an instance of `Num`.” Values whose type lacks the required instance cause the familiar _“No instance for …”_ error.

- **Standard library classes.**
    - `Eq` (equality)
    - `Ord` (total order, requires `Eq`)
    - `Num` (arithmetic)
    - `Show` (string conversion) / `Read` (string parsing) => These form a small hierarchy: e.g. every `Ord` must also be `Eq`.
- **Ad-hoc vs. parametric.** Where parametric functions (like `(++) :: [a] -> [a] -> [a]`) ignore the element type, class-constrained functions choose behaviour per instance (`(==) :: Eq a => a -> a -> Bool`).
# Implementation
### Declaring a class
```haskell
class Eq a where
  (==), (/=) :: a -> a -> Bool
  (x == y) = not (x /= y)    -- default method
  (x /= y) = not (x == y)
```

Defaults mean an instance may define only one of the two methods.
### Making a type an instance
```haskell
data Color = Red | Green

instance Eq Color where
  Red   == Red   = True
  Green == Green = True
  _     == _     = False
```

Missing methods are filled by class defaults. _GHC can derive many standard instances automatically:_
```haskell
data Color = Red | Green
  deriving (Eq, Show)
```
### Using class constraints  
A dictionary keyed by `k` needs equality to find entries:  
```haskell
get :: Eq k => k -> Dict k v -> Maybe v
```

If we optimize using ordering, we strengthen the constraint:
```haskell
get :: Ord k => k -> Dict k v -> Maybe v
```
### Defining your own class  
```haskell
class Default a where
  defValue :: a

instance Default Int    where defValue = 0
instance Default [a]    where defValue = []

get :: (Ord k, Default v) => k -> Dict k v -> v
get _ Empty                = defValue
get key (Bind k v rest)
  | key == k   = v
  | key < k    = defValue
  | otherwise  = get key rest
```

`Default` lets `get` produce a sensible fallback without modifying the dictionary’s shape or API.