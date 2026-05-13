# graph traversal
exploring nodes and edges in a graph to visit each node exactly once or to find a path between two nodes. algorithms like [[💧 Breadth First Search|💧 breadth first search]] and [[🚟 Depth First Search|🚟 depth first search]] are used to traverse graphs.
# theory
a graph is a collection of nodes (or vertices) connected by edges. they can be directed or undirected, and may contain cycles. can be used in problems such as finding the shortest path.
# implementation
```python
class GraphNode:
    def __init__(self, val):
        self.val = val
        self.neighbors = set()

class Graph:
    def __init__(self):
        self.nodes = {}

    def add_node(self, val):
        if val not in self.nodes:
            self.nodes[val] = GraphNode(val)

    def add_edge(self, u_val, v_val, bidirectional=False):
        self.add_node(u_val)
        self.add_node(v_val)
        u, v = self.nodes[u_val], self.nodes[v_val]
        u.neighbors.add(v)
        if bidirectional:
            v.neighbors.add(u)

    def remove_edge(self, u_val, v_val):
	    if u_val in self.nodes and v_val in self.nodes:
	        self.nodes[u_val].neighbors.discard(self.nodes[v_val])
	        self.nodes[v_val].neighbors.discard(self.nodes[u_val])

    def remove_node(self, val):
        if val in self.nodes:
            node_to_remove = self.nodes[val]
            for neighbor in node_to_remove.neighbors:
                neighbor.neighbors.discard(node_to_remove)
            del self.nodes[val]
```
