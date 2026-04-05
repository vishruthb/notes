Essentially we define recursive equations over sets attached to control-flow graph nodes, then iterate until a fixed point. We compute local sets per block, then propagate globally.
# Reaching Definitions
Which assignments *might* arrive here? This is done via forward analysis, taking in all paths that count.

Local sets: 
- `GEN[B]` = definitions created in B. 
- `KILL[B]` = all defs in the entire program that write to the same vars as B's defs whether or not they reach B

Equations: 
- `OUT[B] = GEN[B] ∪ (IN[B] - KILL[B])`
- `IN[B] = ∪ OUT[p]` for predecessors `p`

This is used for dead code elimination and constant propagation, where if all reaching defs of a variable assign the same constant, we replace the use with that constant.
# Dominators
What **must** be passed through to get here? Node A dominates node B if every path from entry to B goes through A. Again we use forward analysis, but the meet op is intersection, meaning that the property must hold on all paths.

Equation: `dom(n) = ∩{dom(m) | m ∈ pred(n)} ∪ {n}`

This is used for building dominator trees, inserting SSA phi-functions, and scoping value numbering.
# Available Expressions
Has this expression already been computed on **all** paths? Forward analysis, meet operator is intersection, meaning that it must be available on _every_ incoming path.

Local sets: 
- DEExpr(b) = expressions computed in b whose operands aren't redefined after (downward exposed). 
- ExprKill(b) = expressions killed by any assignment to their operands in b.

Equation: `Avail(b) = ∩_{x ∈ pred(b)} (DEExpr(x) ∪ (Avail(x) - ExprKill(x)))`
- Initialized via `Avail(entry) = ∅`, and all others start at U (universal set) since we're intersecting.

This is used for global common subexpression elimination. Basically, if `a + b` is available, we reuse the earlier result instead of recomputing.
# Constant Propagation
Is this variable always the same constant? Forward analysis, meet is intersection (must agree on all paths).

Equations (same shape as reaching defs):
- `OUT[S] = GEN[S] ∪ (IN[S] - KILL[S])`
- `IN[S] = ∩_{p ∈ pred(S)} OUT[p]`

We build duild def-use chains (from reaching defs), propagate constants forward, evaluate constant expressions at compile time, and repeat until nothing changes.
# Liveness Analysis
This is just backward analysis that flows from successors to predecessors, and the meet operator is union.

Local sets:
- `UEVAR(b)` = variables used in b before being defined in b (upward exposed)
- `VARKILL(b)` = variables assigned/defined in b.

Equations:
- `LIVEOUT(b) = ∪_{s ∈ succ(b)} LIVEIN(s)`
- `LIVEIN(b) = UEVAR(b) ∪ (LIVEOUT(b) - VARKILL(b))`

This is mainly used for register allocation. two variables that are live at the same time interfere and can't share a register.
# Computation
Taking everything from above, the formalized steps are:
1. Compute local sets (GEN/KILL or UEVAR/VARKILL or DEExpr/ExprKill) for each block
2. Initialize IN/OUT sets (∅ for entry/exit boundary, ∅ or U for others depending on meet operator – ∅ for union-based, U for intersection-based)
3. For each block, recompute IN then OUT (or OUT then IN for backward) using the equations
4. Stop when nothing changes -> fixed point
