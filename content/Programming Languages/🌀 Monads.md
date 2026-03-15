# Theory
- A _monad_ is a type constructor `m` plus two combinators that let you run one computation after another while hiding the low-level plumbing.
- Laws:
    - `return :: a -> m a` – inject a pure value.
    - `(>>=) :: m a -> (a -> m b) -> m b` – pass the result of step 1 into step 2 (a.k.a. _bind_).
- Consequence:
    - **Error handling**: `Either e a` stops on the first `Left`.
    - **State threading**: `State s a` passes new state forward.
    - **Non-determinism**: `[a]` explores every possibility.
    - **Input/Output**: `IO a` talks to the outside world safely.
- **Do-notation** `do { x ← m1; y ← m2; … }` is just pretty syntax for nested `>>=` chains, making monadic code read top-to-bottom.
# Implementation
```haskell
-- Generic interface
class Monad m where
  (>>=)  :: m a -> (a -> m b) -> m b
  return :: a -> m a
```
### Example: Error Monad (`Either`)
```haskell
type Result a = Either String a     -- alias

instance Monad Result where
  Left  err >>= _ = Left err        -- abort on first error
  Right v  >>= f = f v              -- continue on success
  return = Right
```

_Using it with `do`_:
```haskell
eval (Plus e1 e2) = do
  v1 <- eval e1        -- Result Int
  v2 <- eval e2
  return (v1 + v2)
```

The `(>>=)` instance removed every explicit `case` on `Either`.
### Swapping effects
Just switch a type alias—the function bodies stay identical:
```haskell
type Interpreter a = State Int a   -- counts operations
-- or
type Interpreter a = [a]           -- explores branches
```

Different monads choose _how_ steps are sequenced without touching the high-level logic.