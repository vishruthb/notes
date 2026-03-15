# Theory
- **What is a parser?** A function that converts raw input (text, bytes …) into a structured value—e.g. an AST—while leaving the unconsumed suffix for later stages.
- **Type model.** `haskell data Parser a = P (String -> [(a,String)])`  
    _List_ result supports _nondeterminism_ (many possible parses) and an empty list means failure.
- **Composability problem.** Hand-rolled regexes or parser-generator grammars don’t compose; parser _combinators_ treat parsers as first-class values so you can build big parsers out of small ones.
- **Monads(-ish).** `Parser` behaves like `State + []`: with `return` (inject) and `>>=` (bind) you sequence steps while threading the remaining input and branching over alternatives. Do-notation then hides the plumbing.
# Implementation
### Core primitives
```haskell
runParser :: Parser a -> String -> [(a,String)]
runParser (P f) s = f s

oneChar :: Parser Char        -- succeeds on any single char
oneChar = P $ \s -> case s of
                      c:cs -> [(c,cs)]
                      []   -> []
```
### Monad instance  
```haskell
returnP a        = P $ \s -> [(a,s)]
bindP pa k       = P $ \s ->
  concatMap (\(a,s') -> runParser (k a) s') (runParser pa s)

instance Monad Parser where
  return = returnP
  (>>=)  = bindP
```
### Useful combinators  
```haskell
failP :: Parser a
failP = P $ const []

(<|>) :: Parser a -> Parser a -> Parser a  -- choice
p1 <|> p2 = P $ \s -> case runParser p1 s of
                        []  -> runParser p2 s
                        r1s -> r1s
```

`manyP` repeats a parser zero-or-more times; `manyOneP` requires at least one.
### Higher-level builders
```haskell
-- Left-associative fold of vP separated by oP
foldLP vP oP = do
  v1 <- vP
  let step acc = (do o <- oP; v <- vP; step (acc `o` v))
                 <|> return acc
  step v1
```

Used to respect associativity (`expr = foldLP int opP`). Precedence is handled by _grammar factoring_:
```haskell
expr = foldLP prod (plus <|> minus)
prod = foldLP atom (times <|> divide)
atom = parensP expr <|> int
```