# shortest path problem
find the shortest path from a source node to all other nodes in a graph with non-negative edge weights.
# theory
use a priority queue to efficiently extract the node with the smallest tentative distance and updates the distances of its neighbors if a shorter path is found.
## key operations:
- **`deletemin(H)`**: retrieves the vertex with the smallest distance from the priority queue.
- **`decreasekey(H, u)`**: updates a vertex's distance in the queue if a shorter path is found.
# implementation
```python
import heapq

def dijkstra(graph, source):
    """
    Parameters:
        graph (dict): Adjacency list representation {node: [(neighbor, weight)]}.
        source (int): The starting node.

    Returns:
        dict: Shortest distances from the source to all other nodes.
    """
    # Initialize distances and priority queue
    dist = {node: float('inf') for node in graph}
    dist[source] = 0
    pq = [(0, source)]  # (distance, node)

    while pq:
        current_dist, current_node = heapq.heappop(pq)  # deletemin operation

        # Skip processing if this distance is not up-to-date
        if current_dist > dist[current_node]:
            continue

        # Explore neighbors
        for neighbor, weight in graph[current_node]:
            new_dist = current_dist + weight
            if new_dist < dist[neighbor]:  # Found a shorter path
                dist[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))  # decreasekey operation

    return dist
```
## runtime
- **priority queue operations**:
	- `deletemin`: $O(\log V)$
	- `decreasekey`: $(O(\log V)$
- **overall complexity**:
	- binary heap: $(O((V + E) \log V)$
	- array (naive): $O(V^2)$
