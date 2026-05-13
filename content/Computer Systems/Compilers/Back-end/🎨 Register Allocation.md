ir assumes unlimited virtual registers, while real machines only have some $k$ amount of physical registers. we need to map virtual -> physical. if two variables are "live"/in use at the same time, they cannot share a register.
# liveness analysis
we work backwards through the code. a variable is live at a point if its current value might be used variable before being overwritten. at each instruction, we compute the set of live variables.

example: just before an instruction `x = a + b`, `a` and `b` become live because they are being used. `x` stop being alive above this point, as it is being defined here, and its old value is dead.
# interference graphs
here, nodes = variables. we draw an edge between two variables if they are live at the same time at any point. if two variables interfere, then they can't share a register.
# chaitin's algorithm
aka graph coloring. we need to $k$-color the graph, where $k$ is the number of physical registers. the algorithm goes as:
1. simplify by finding any node with fewer than k neighbors. remove it and push it on a stack. this works because if a node has < $k$ neighbors, we can always find a color for it later, no matter what colors the neighbor gets.
2. repeat until the graph is empty or every remaining node has $\geq k$ neighbors.
3. if we get stuck, where all nodes have $\geq k$ neighbors, we **spill**. spilling is where we pick a node and decide to store that variable **in memory instead**. we remove it and contain. we can also use brigg's optimistic approach, where we push it on the stack anyway and hope that a color is available when we pop.
4. pop nodes off the stack one by one, assign each the lowest color not used by its currently colored niehgbors.

if spilling occurred, we can insert load/store instructions around the spilled variable's uses and definitions, then rerun the whole process.