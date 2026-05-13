# theory
a _closure_ is a first-class function bundled together with the environment that was in scope when the function was **defined**. that frozen environment guarantees that every free variable inside the function body continues to point to the value it originally captured, no matter where or when the function is later called.

closures are the key mechanism that enforces **static (lexical) scoping**. under lexical scoping, each variable use is resolved to the _nearest_ binding in the program text, producing referential transparency (the same expression always yields the same value). without the preserved environment, calls would fall back on whatever bindings happen to be live at run time—dynamic scoping—which breaks that guarantee.

because the environment travels with the code, closures naturally support higher-order patterns such as partial application (`let add1 = add 1`) and functions that consume or produce other functions (`doTwice inc`). each of these examples works because captured variables like `x` or `f` retain their original bindings inside every call.
# implementation
below is a minimal structural recipe—illustrated in haskell-like pseudocode—for turning an interpreter that already supports numbers, variables, and `let` into one that handles closures.

```haskell
-- 1 ▸ Extend the Value type
data Value
  = VNum  Int
  | VClos Env Id Expr      -- ⟨frozenEnv , parameter , body⟩

type Env = [(Id, Value)]

-- 2 ▸ Building a closure (λ-abstraction)
eval env (Lam x body) =
  VClos env x body         -- freeze *current* env

-- 3 ▸ Calling a function (application)
eval env (App e1 e2) =
  case eval env e1 of
    VClos frozen param body ->
      let v2   = eval env   e2
          env' = (param, v2) : frozen   -- extend the *frozen* env
      in  eval env' body
    _ -> error "attempt to call a non-function"
```