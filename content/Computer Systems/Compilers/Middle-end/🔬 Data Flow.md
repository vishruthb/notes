essentially we define recursive equations over sets attached to control-flow graph nodes, then iterate until a fixed point. we compute local sets per block, then propagate globally.
# reaching definitions
which assignments *might* arrive here? this is done via forward analysis, taking in all paths that count.

local sets:
- `GEN[B]` = definitions created in b.
- `KILL[B]` = all defs in the entire program that write to the same vars as b's defs whether or not they reach b

equations:
- `OUT[B] = GEN[B] ∪ (IN[B] - KILL[B])`
- `IN[B] = ∪ OUT[p]` for predecessors `p`

this is used for dead code elimination and constant propagation, where if all reaching defs of a variable assign the same constant, we replace the use with that constant.
# dominators
what **must** be passed through to get here? node a dominates node b if every path from entry to b goes through a. again we use forward analysis, but the meet op is intersection, meaning that the property must hold on all paths.

equation: `dom(n) = ∩{dom(m) | m ∈ pred(n)} ∪ {n}`

this is used for building dominator trees, inserting ssa phi-functions, and scoping value numbering.
# available expressions
has this expression already been computed on **all** paths? forward analysis, meet operator is intersection, meaning that it must be available on _every_ incoming path.

local sets:
- deexpr(b) = expressions computed in b whose operands aren't redefined after (downward exposed).
- exprkill(b) = expressions killed by any assignment to their operands in b.

equation: `Avail(b) = ∩_{x ∈ pred(b)} (DEExpr(x) ∪ (Avail(x) - ExprKill(x)))`
- initialized via `Avail(entry) = ∅`, and all others start at u (universal set) since we're intersecting.

this is used for global common subexpression elimination. basically, if `a + b` is available, we reuse the earlier result instead of recomputing.
# constant propagation
is this variable always the same constant? forward analysis, meet is intersection (must agree on all paths).

equations (same shape as reaching defs):
- `OUT[S] = GEN[S] ∪ (IN[S] - KILL[S])`
- `IN[S] = ∩_{p ∈ pred(S)} OUT[p]`

we build duild def-use chains (from reaching defs), propagate constants forward, evaluate constant expressions at compile time, and repeat until nothing changes.
# liveness analysis
this is just backward analysis that flows from successors to predecessors, and the meet operator is union.

local sets:
- `UEVAR(b)` = variables used in b before being defined in b (upward exposed)
- `VARKILL(b)` = variables assigned/defined in b.

equations:
- `LIVEOUT(b) = ∪_{s ∈ succ(b)} LIVEIN(s)`
- `LIVEIN(b) = UEVAR(b) ∪ (LIVEOUT(b) - VARKILL(b))`

this is mainly used for register allocation. two variables that are live at the same time interfere and can't share a register.
# computation
taking everything from above, the formalized steps are:
1. compute local sets (gen/kill or uevar/varkill or deexpr/exprkill) for each block
2. initialize in/out sets (∅ for entry/exit boundary, ∅ or u for others depending on meet operator – ∅ for union-based, u for intersection-based)
3. for each block, recompute in then out (or out then in for backward) using the equations
4. stop when nothing changes -> fixed point
