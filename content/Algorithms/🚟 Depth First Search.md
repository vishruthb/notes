# traversal problem
visit each vertex connected to a source node in a graph, ensuring all nodes are reached and detecting any cycles if they exist.
# theory
explore as far down a branch as possible before backtracking, using a recursive approach and a visited set to ensure each node is processed only once.
# implementation
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

# runtime
$$O(V+E)$$
dfs visits each vertex and edge once, leading to a linear time complexity relative to the number of vertices $V$ and edges $E$. space complexity is $O(V)$ due to the recursion stack and visited array.

# other notes
## terms
- **pre/post numbers**: when nodes are entered/exited
- **dfs forest**: represents tree relationships
- **tree/forward edges**: go to descendants
- **back edges**: return to ancestors => indicate cycles in directed graphs
- **cross edges**: connect two unrelated dfs branches
- **topological sort**: running dfs and ordering nodes by decreasing post numbers. guarantees that all edges go forward.