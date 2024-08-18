# Graph Traversal
Exploring nodes and edges in a graph to visit each node exactly once or to find a path between two nodes. Algorithms like [[💧 Breadth First Search]] and [[🚟 Depth First Search]] are used to traverse graphs.

# Theory
A graph is a collection of nodes (or vertices) connected by edges. They can be directed or undirected, and may contain cycles. Can be used in problems such as finding the shortest path.

# Implementation
```python
class Graph:
    def __init__(self):
        self.graph = {}

    def add_edge(self, u, v):
        if u in self.graph:
            self.graph[u].append(v)
        else:
            self.graph[u] = [v]
```