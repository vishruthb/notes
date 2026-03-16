We have intermediate representations (IR) that are three-address codes and need to map it to actual machine instructions. Problem is that multiple machine instructions could implement the same IR, and some combinations are cheaper than others.
# Peephole Matching
Slide a small window over the IR, and look for patterns that we can replace with **better instructions**.

Example: a `store` followed by a `load` of the same address, could just be a register copy instead.
# Tree Matching
Convert the IR into a tree (expansion tree) where edges represent value flow. Then cover/tile the three with patterns, where each tile corresponds to one machine instruction. Multiple tilings are possible, we just want the cheapest one.

To find good tilings, we could either use greedy - start at the root and pick the biggest/cheapest tile that fits, then recurse on uncovered trees), or dynamic programming - bottom-up and compute optimal cost for each subtree, memoize, and combine. DP gives optimal results.