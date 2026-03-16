Different instructions take different number of cycles (latencies). If instruction B depends on instruction A's result, and A takes e.g. 3 cycles, then B must wait. The scheduler reorders instructions to fill those wait cycles with useful work.
# Dependence Graph
Nodes = instructions. We draw an edge from A to B if B depends on A (uses A's result). We label each edge with A's latency. There are three types of dependencies:
- True dependence (RAW): A writes x, B reads x
- Anti dependence (WAR): A reads x, B writes x
- Output dependence (WAW): A writes x, B writes x
# Critical Path
The critical path of a node is the **longest weighted path from that node to any leaf**. We compute bottom-up using DFS. Leaf nodes have a critical path = 0. For a node n with edges to children, critical path(n) = max over children c of latency(n -> c) + critical path(c).
# List Scheduling
Use the critical path as priority, where a higher critical path means higher priority meaning that we schedule it first. Then, for each cycle:
- Look at the ready list - instructions whose predecessors are all done and latencies have elapsed
- Pick the ready instruction with the highest critical path
- Issue it and add to the active list
- If nothing is ready, stall (insert nop)

# Example
```
A: loadI 5 → r1       (latency 1)
B: load x → r2        (latency 3)
C: add r1, r2 → r3    (latency 1)
D: loadI 7 → r4       (latency 1)
E: load y → r5        (latency 3)
F: mult r4, r5 → r6   (latency 4)
G: add r3, r6 → r7    (latency 1)
```

Naively, this order has 14 cycles with a lot of stalls. However, after list scheduling, we can get it down to 8 cycles. We do this by moving E and D earlier to fill stall slots while waiting for B's load to complete.