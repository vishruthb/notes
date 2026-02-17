# Shortest Path Problem
Find the shortest path from a source node to all other nodes in a graph with non-negative edge weights.

# Theory
Use a priority queue to efficiently extract the node with the smallest tentative distance and updates the distances of its neighbors if a shorter path is found.

## Key Operations:
1. **`deletemin(H)`**: Retrieves the vertex with the smallest distance from the priority queue.
2. **`decreasekey(H, u)`**: Updates a vertex's distance in the queue if a shorter path is found.

# Implementation
```python
import heapq

def dijkstra(graph, source):
    """
    Finds the shortest path from the source to all vertices in the graph.

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

## Runtime
1. **Priority Queue Operations**:
   - `deletemin`: $O(\log V)$
   - `decreasekey`: $(O(\log V)$
2. **Overall Complexity**:
   - Binary Heap: $(O((V + E) \log V)$
   - Array (naive): $O(V^2)$
