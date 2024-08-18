# Traversal Problem
Visit each vertex connected to a source node in a graph, ensuring all nodes are reached and detecting any cycles if they exist.
# Theory
Explore as far down a branch as possible before backtracking, using a recursive approach and a visited set to ensure each node is processed only once.
# Implementation
```python
def dfs(s, adj):
	vis = set()
	dfs_visit(s, adj, vis)
 
def dfs_visit(s, adj, vis):
	vis.add(s)
	for n in adj[s]:
		if n not in vis:
			dfs_visit(n, adj, vis)
```

# Runtime
$$O(V+E)$$
DFS visits each vertex and edge once, leading to a linear time complexity relative to the number of vertices $V$ and edges $E$. Space complexity is $O(V)$ due to the recursion stack and visited array.