# Connected Component Problem
Detect cycles and track connected components in a dynamic graph through maintaining a series of disjoint sets.
# Theory
Union-Find maintains a collection of disjoint sets and provides two main operations: `find`, which identifies the root representative of a set, and `union`, which merges two sets. Initially, each node is its own parent. When a union occurs, the smaller tree is attached under the larger one to keep the structure balanced, a process called union by rank. Additionally, path compression optimizes the `find` operation by linking nodes directly to their root, flattening the tree structure. This ensures nearly constant-time operations and makes Union-Find an intuitive approach for detecting cycles and maintaining connectivity in dynamic graphs.
# Implementation
```python
class UnionFind:
    def __init__(self, n):
        self.par = {i: i for i in range(1, n + 1)}  # Parent mapping
        self.rank = {i: 0 for i in range(1, n + 1)}  # Rank for balancing

    def find(self, n):
        if self.par[n] != n:
            self.par[n] = self.find(self.par[n])  # Path compression
        return self.par[n]
    
    def union(self, n1, n2):
        p1, p2 = self.find(n1), self.find(n2)
        if p1 == p2:
            return False  # Cycle detected
        
        if self.rank[p1] > self.rank[p2]:
            self.par[p2] = p1
        elif self.rank[p1] < self.rank[p2]:
            self.par[p1] = p2
        else:
            self.par[p1] = p2
            self.rank[p2] += 1
        return True
```
# Runtime
`find`: $O(n)$ in the naive case, in most cases $O(\alpha(n))$ with path compression.
`union`: $O(\alpha(n))$, where $\alpha$ is the Inverse Ackermann function. For nearly all input sizes, it is pretty much $O(1)$.

Overall, given $m$ edges, resulting complexity of Union-Find is $O(m * \alpha(n)) \rightarrow O(m)$.