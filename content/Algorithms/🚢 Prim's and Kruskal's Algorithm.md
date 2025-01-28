# Minimum Spanning Tree (MST) Problem
Find the minimum spanning tree of a connected graph, ensuring that all vertices are connected with the minimum total edge weight.

## Theory
A Minimum Spanning Tree (MST) connects all vertices of a graph with the smallest possible total edge weight, without creating any cycles. Two common algorithms for solving the MST problem are **Kruskal's Algorithm** and **Prim's Algorithm**.

### **Kruskal’s Algorithm**
A greedy approach that processes edges in ascending order of weights and adds them to the MST if they do not form a cycle.

### **Prim’s Algorithm**
Another greedy approach that grows the MST by starting from any arbitrary node and adding the smallest edge that connects a new vertex to the current tree.

# Implementation
## Kruskal’s Algorithm

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, u):
        if self.parent[u] != u:
            self.parent[u] = self.find(self.parent[u])  # Path compression
        return self.parent[u]

    def union(self, u, v):
        root_u = self.find(u)
        root_v = self.find(v)
        if root_u != root_v:
            if self.rank[root_u] > self.rank[root_v]:
                self.parent[root_v] = root_u
            elif self.rank[root_u] < self.rank[root_v]:
                self.parent[root_u] = root_v
            else:
                self.parent[root_v] = root_u
                self.rank[root_u] += 1


def kruskal(graph, num_vertices):
    """
    Finds the minimum spanning tree using Kruskal's algorithm.

    Parameters:
        graph (list): List of edges [(u, v, weight)].
        num_vertices (int): Number of vertices in the graph.

    Returns:
        list: Edges in the minimum spanning tree.
        int: Total weight of the MST.
    """
    uf = UnionFind(num_vertices)
    mst = []
    total_weight = 0

    # Sort edges by weight
    graph.sort(key=lambda x: x[2])

    for u, v, weight in graph:
        if uf.find(u) != uf.find(v):  # No cycle
            uf.union(u, v)
            mst.append((u, v, weight))
            total_weight += weight

    return mst, total_weight
```

### Runtime:
- Sorting Edges: $(O(E \log E)$
- Union-Find Operations: $O(E \cdot \alpha(V))$, where $\alpha$ is the inverse Ackermann function.
- **Overall**: $O(E \log E)$

## Prim’s Algorithm

```python
import heapq

def prim(graph, num_vertices):
    """
    Finds the minimum spanning tree using Prim's algorithm.

    Parameters:
        graph (dict): Adjacency list representation {node: [(neighbor, weight)]}.
        num_vertices (int): Number of vertices in the graph.

    Returns:
        list: Edges in the minimum spanning tree.
        int: Total weight of the MST.
    """
    visited = [False] * num_vertices
    mst = []
    total_weight = 0
    pq = [(0, 0, -1)]  # (weight, current_node, parent_node)

    while pq and len(mst) < num_vertices - 1:
        weight, current_node, parent = heapq.heappop(pq)
        if visited[current_node]:
            continue

        visited[current_node] = True
        if parent != -1:
            mst.append((parent, current_node, weight))
            total_weight += weight

        for neighbor, edge_weight in graph[current_node]:
            if not visited[neighbor]:
                heapq.heappush(pq, (edge_weight, neighbor, current_node))

    return mst, total_weight
```

### Runtime:
- Priority Queue Operations: $O(E \log V)$
- **Overall**: $(O(E \log V)$

# Notes

| **Aspect**     | **Kruskal's**  | **Prim's**                 |
| -------------- | -------------- | -------------------------- |
| Approach       | Greedy by edge | Greedy by growing the tree |
| Data Structure | Union-Find     | Priority Queue             |
| Best for       | Sparse graphs  | Dense graphs               |
| Runtime        | $O(E \log E)$  | $O(E \log V)$              |

