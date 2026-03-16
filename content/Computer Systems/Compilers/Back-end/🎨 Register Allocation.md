IR assumes unlimited virtual registers, while real machines only have some $k$ amount of physical registers. We need to map virtual -> physical. If two variables are "live"/in use at the same time, they cannot share a register.
# Liveness Analysis
We work backwards through the code. A variable is live at a point if its current value might be used variable before being overwritten. At each instruction, we compute the set of live variables.

Example: Just before an instruction `x = a + b`, `a` and `b` become live because they are being used. `x` stop being alive above this point, as it is being defined here, and its old value is dead.
# Interference Graphs
Here, nodes = variables. We draw an edge between two variables if they are live at the same time at any point. If two variables interfere, then they can't share a register.
# Chaitin's Algorithm
Aka graph coloring. We need to $k$-color the graph, where $k$ is the number of physical registers. The algorithm goes as:
1. Simplify by finding any node with fewer than k neighbors. Remove it and push it on a stack. This works because if a node has < $k$ neighbors, we can always find a color for it later, no matter what colors the neighbor gets.
2. Repeat until the graph is empty or every remaining node has $\geq k$ neighbors.
3. If we get stuck, where all nodes have $\geq k$ neighbors, we **spill**. Spilling is where we pick a node and decide to store that variable **in memory instead**. We remove it and contain. We can also use Brigg's optimistic approach, where we push it on the stack anyway and hope that a color is available when we pop.
4. Pop nodes off the stack one by one, assign each the lowest color not used by its currently colored niehgbors.

If spilling occurred, we can insert load/store instructions around the spilled variable's uses and definitions, then rerun the whole process.