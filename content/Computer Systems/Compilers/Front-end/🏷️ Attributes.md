attribute grammars let us attach computations to the parse tree created in the [[📄 Parsing|📄 parsing]] stage to handle semantic analysis, which can be used to catch things like "variable used before declaration" or "type mismatch in assignment".

essentially, we take a cfg and attach attributes (named values) to each node in the parse tree + rules that define how to compute those attributes. these rules are tied to productions.
# types of attributes
- synthesized attributes: flow upward in the parse tree, where parent's attribute are computed from children's attribute.
- inherited attributes: flow downward in the parse tree, where a child's attribute is computed from its parent or siblings.
# example
consider this grammar for simple addition expressions.
```
Expr → Expr + Term
Expr → Term
Term → 0 | 1 | 2
```
with these attribution rules:
- `Expr → Expr₁ + Term`: expr.val = expr1.val + term.val
- `Expr → Term`: expr.val = term.val
- `Term → 0`: term.val = 0 (same pattern for 1, 2)

for the input `1 + 2 + 0`:
we know that `val` is synthesized as it only depends on children's values. knowing that, we can then build a parse tree and annotate every node with its `val`, giving us:

```
        Expr          val = 3
       / | \
    Expr  +  Term     val = 0
    / | \          |
 Expr + Term   0
  |           |
Term       2     val = 2
  |
  1       val = 1
```

a valid evaluation order for this tree would look like:
- term(1).val = 1
- expr(bottom).val = 1
- term(2).val = 2
- expr(middle).val = 1 + 2 = 3
- term(0).val = 0
- expr(top).val = 3 + 0 = **3**

other valid orders do exist, such as computing term(0).val = 0 as step 1 since it has no deps, but we must respect the constraint that a node's val can't be computed until its children's vals are done.