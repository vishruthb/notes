we have intermediate representations (ir) that are three-address codes and need to map it to actual machine instructions. problem is that multiple machine instructions could implement the same ir, and some combinations are cheaper than others.
# peephole matching
slide a small window over the ir, and look for patterns that we can replace with **better instructions**.

example: a `store` followed by a `load` of the same address, could just be a register copy instead.
# tree matching
convert the ir into a tree (expansion tree) where edges represent value flow. then cover/tile the three with patterns, where each tile corresponds to one machine instruction. multiple tilings are possible, we just want the cheapest one.

to find good tilings, we could either use greedy - start at the root and pick the biggest/cheapest tile that fits, then recurse on uncovered trees), or dynamic programming - bottom-up and compute optimal cost for each subtree, memoize, and combine. dp gives optimal results.