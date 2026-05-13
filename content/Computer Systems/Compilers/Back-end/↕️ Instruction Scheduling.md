different instructions take different number of cycles (latencies). if instruction b depends on instruction a's result, and a takes e.g. 3 cycles, then b must wait. the scheduler reorders instructions to fill those wait cycles with useful work.
# dependence graph
nodes = instructions. we draw an edge from a to b if b depends on a (uses a's result). we label each edge with a's latency. there are three types of dependencies:
- true dependence (raw): a writes x, b reads x
- anti dependence (war): a reads x, b writes x
- output dependence (waw): a writes x, b writes x
# critical path
the critical path of a node is the **longest weighted path from that node to any leaf**. we compute bottom-up using dfs. leaf nodes have a critical path = 0. for a node n with edges to children, critical path(n) = max over children c of latency(n -> c) + critical path(c).
# list scheduling
use the critical path as priority, where a higher critical path means higher priority meaning that we schedule it first. then, for each cycle:
- look at the ready list - instructions whose predecessors are all done and latencies have elapsed
- pick the ready instruction with the highest critical path
- issue it and add to the active list
- if nothing is ready, stall (insert nop)
# example
```
A: loadI 5 → r1       (latency 1)
B: load x → r2        (latency 3)
C: add r1, r2 → r3    (latency 1)
D: loadI 7 → r4       (latency 1)
E: load y → r5        (latency 3)
F: mult r4, r5 → r6   (latency 4)
G: add r3, r6 → r7    (latency 1)
```

naively, this order has 14 cycles with a lot of stalls. however, after list scheduling, we can get it down to 8 cycles. we do this by moving e and d earlier to fill stall slots while waiting for b's load to complete.