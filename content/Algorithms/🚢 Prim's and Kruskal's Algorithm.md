# minimum spanning tree (mst) problem
find the minimum spanning tree of a connected graph, ensuring that all vertices are connected with the minimum total edge weight.

## theory
a minimum spanning tree (mst) connects all vertices of a graph with the smallest possible total edge weight, without creating any cycles. two common algorithms for solving the mst problem are **kruskal's algorithm** and **prim's algorithm**.

### **kruskal’s algorithm**
a greedy approach that processes edges in ascending order of weights and adds them to the mst if they do not form a cycle.

### **prim’s algorithm**
another greedy approach that grows the mst by starting from any arbitrary node and adding the smallest edge that connects a new vertex to the current tree.

# implementation
## kruskal’s algorithm

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

### runtime:
- sorting edges: $(O(E \log V)$
- union-find operations: $O(E \cdot \alpha(V))$, where $\alpha$ is the inverse ackermann function. find takes the height of the tree, which we can prove to be up to $O(\lg(n))$, where $n$ is the number of vertices $|V|$.
- **overall**: $O(E \log V)$

## prim’s algorithm

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

### runtime:
- priority queue operations: $O(E \log V)$
- **overall**: $(O(E \log V)$

# notes

| **aspect**     | **kruskal's**  | **prim's**                 |
| -------------- | -------------- | -------------------------- |
| approach       | greedy by edge | greedy by growing the tree |
| data structure | union-find     | priority queue             |
| best for       | sparse graphs  | dense graphs               |
| runtime        | $O(E \log E)$  | $O(E \log V)$              |

