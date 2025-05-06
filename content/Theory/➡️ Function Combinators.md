# Theory  
Combinators are functions that manipulate functions.

| combinator | type | purpose |
|------------|------|---------|
| `(.)` | $$ (b \to c) \to (a \to b) \to a \to c $$ | composition |
| `flip` | $$ (a \to b \to c) \to b \to a \to c $$ | swap arguments |
| `($)` | $$ (a \to b) \to a \to b $$ | low‑precedence application |
| `id` | $$ a \to a $$ | identity 
### Eta-conversion
- **Expansion**: $f \;\Longrightarrow\; \lambda x.\, f\,x$  
- **Contraction**: $\lambda x.\, f\,x \;\Longrightarrow\; f$ (when $x$ not free in $f$)
# Implementation
```haskell
-- Point‑free rewrite
sumOfSquares = sum . map (^2)

-- Re‑order for foldl
foldl (flip (:)) [] xs   ==   reverse xs
```