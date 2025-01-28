# Traversal Problem
Visit each vertex connected to a source node in a graph, ensuring all nodes are reached and detecting any cycles if they exist.
# Theory
Explore as far down a branch as possible before backtracking, using a recursive approach and a visited set to ensure each node is processed only once.
# Implementation
```python
# adjacency list
def dfs(s, adj):
	vis = set()
	dfs_visit(s, adj, vis)
 
def dfs_visit(s, adj, vis):
	vis.add(s)
	for n in adj[s]:
		if n not in vis:
			dfs_visit(n, adj, vis)

# matrix [time: O(4^(n*m)), space: O(n*m)]
def dfs(grid, r, c, visit):
    ROWS, COLS = len(grid), len(grid[0])
    if (min(r, c) < 0 or
        r == ROWS or c == COLS or
        (r, c) in visit or grid[r][c] == 1):
        return 0
    if r == ROWS - 1 and c == COLS - 1:
        return 1

    visit.add((r, c))

    count = 0
    count += dfs(grid, r + 1, c, visit)
    count += dfs(grid, r - 1, c, visit)
    count += dfs(grid, r, c + 1, visit)
    count += dfs(grid, r, c - 1, visit)

    visit.remove((r, c))
    return count
```

# Runtime
$$O(V+E)$$
DFS visits each vertex and edge once, leading to a linear time complexity relative to the number of vertices $V$ and edges $E$. Space complexity is $O(V)$ due to the recursion stack and visited array.

# Other Notes
## Terms
- **Pre/post numbers**: When nodes are entered/exited
- **DFS Forest**: Represents tree relationships
- **Tree/Forward Edges**: Go to descendants
- **Back Edges**: Return to ancestors => Indicate cycles in directed graphs
- **Cross Edges**: Connect two unrelated DFS branches
- **Topological Sort**: Running DFS and ordering nodes by decreasing post numbers. Guarantees that all edges go forward.