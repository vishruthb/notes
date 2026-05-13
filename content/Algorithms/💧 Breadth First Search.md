# shortest path problem
visit each vertex connected to a source node in a graph, finding the shortest path from the source to all other nodes in an unweighted graph.
# theory
use a queue to explore vertices level by level, ensuring that the shortest path to each vertex is found by processing nodes in ascending distance order.
# implementation
```python
# adjacency list
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

# matrix
def bfs(grid):
    ROWS, COLS = len(grid), len(grid[0])
    visit = set()
    queue = deque()
    queue.append((0, 0))
    visit.add((0, 0))

    length = 0
    while queue:
        for i in range(len(queue)):
            r, c = queue.popleft()
            if r == ROWS - 1 and c == COLS - 1:
                return length

            neighbors = [[0, 1], [0, -1], [1, 0], [-1, 0]]
            for dr, dc in neighbors:
                if (min(r + dr, c + dc) < 0 or
                    r + dr == ROWS or c + dc == COLS or
                    (r + dr, c + dc) in visit or grid[r + dr][c + dc] == 1):
                    continue
                queue.append((r + dr, c + dc))
                visit.add((r + dr, c + dc))
        length += 1
```
# runtime
$$O(V+E)$$
bfs visits each vertex and edge exactly once, resulting in a linear time complexity relative to the number of vertices $V$ and edges $E$. space complexity is $O(V)$ due to the queue and distance dictionary.