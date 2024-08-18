# Traversal Problem
Visit each vertex connected to a source node in a graph, finding the shortest path from the source to all other nodes in an unweighted graph.

# Theory
Use a queue to explore vertices level by level, ensuring that the shortest path to each vertex is found by processing nodes in ascending distance order.

# Implementation
```python
from collections import deque

def bfs(s, adj):
    q = deque([s])
    dist = {s: 0}
    while q:
        c = q.popleft()
        for n in adj[c]:
            if n not in dist:
                dist[n] = dist[c] + 1
                q.append(n)
```

# Runtime
$$O(V+E)$$
BFS visits each vertex and edge exactly once, resulting in a linear time complexity relative to the number of vertices $V$ and edges $E$. Space complexity is $O(V)$ due to the queue and distance dictionary.