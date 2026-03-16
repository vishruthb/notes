Attribute grammars let us attach computations to the parse tree created in the [[📄 Parsing]] stage to handle semantic analysis, which can be used to catch things like "variable used before declaration" or "type mismatch in assignment".

Essentially, we take a CFG and attach attributes (named values) to each node in the parse tree + rules that define how to compute those attributes. These rules are tied to productions.

# Types of Attributes
- Synthesized attributes: flow upward in the parse tree, where parent's attribute are computed from children's attribute.
- Inherited attributes: flow downward in the parse tree, where a child's attribute is computed from its parent or siblings.

# Example
Consider this grammar for simple addition expressions.
```
Expr → Expr + Term
Expr → Term
Term → 0 | 1 | 2
```
with these attribution rules:
- `Expr → Expr₁ + Term`: Expr.val = Expr1.val + Term.val
- `Expr → Term`: Expr.val = Term.val
- `Term → 0`: Term.val = 0 (same pattern for 1, 2)

For the input `1 + 2 + 0`:
We know that `val` is synthesized as it only depends on children's values. Knowing that, we can then build a parse tree and annotate every node with its `val`, giving us:

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

A valid evaluation order for this tree would look like:
- Term(1).val = 1
- Expr(bottom).val = 1
- Term(2).val = 2
- Expr(middle).val = 1 + 2 = 3
- Term(0).val = 0
- Expr(top).val = 3 + 0 = **3**

Other valid orders do exist, such as computing Term(0).val = 0 as step 1 since it has no deps, but we must respect the constraint that a node's val can't be computed until its children's vals are done.