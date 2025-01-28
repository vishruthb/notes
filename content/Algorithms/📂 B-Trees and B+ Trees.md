# Scalable Sorted Data Access Problem
Efficiently store and manage large datasets in a sorted manner while minimizing disk I/O operations. These trees are particularly useful for database indexing and filesystem implementations.
# Theory
**B-Trees** and **B+ Trees** are generalizations of binary search trees designed to handle large amounts of data stored on disk. They differ as follows:
## B-Trees:
- Internal nodes store both keys and pointers to child nodes.
- Keys guide searches, and data can be stored in internal or leaf nodes.
- Supports efficient search, insertion, and deletion while maintaining balance.
## B+ Trees:
- All data is stored only in the leaf nodes, and internal nodes store only keys for guiding searches.
- Leaf nodes are linked to form a sorted doubly linked list, making range queries efficient.
- More efficient for sequential access.
## Common Properties:
1. Nodes can have multiple children (defined by order `m`):
   - An internal node can have up to `m - 1` keys and `m` children.
   - A node must have at least `⌈m/2⌉` children (except the root).
2. Trees remain balanced by redistributing keys or splitting/merging nodes as needed.
3. Height remains logarithmic: $O(\log_m(n))$.
# Implementation
```python
class BTreeNode:
    def __init__(self, t, leaf=False):
        self.t = t  # Minimum degree
        self.leaf = leaf
        self.keys = []  # Keys in this node
        self.children = []  # Child pointers

class BTree:
    def __init__(self, t):
        self.root = BTreeNode(t, True)
        self.t = t

    def _split_child(self, parent, i, full_node):
        t = self.t
        new_node = BTreeNode(t, full_node.leaf)
        parent.children.insert(i + 1, new_node)
        parent.keys.insert(i, full_node.keys[t - 1])

        new_node.keys = full_node.keys[t:]
        full_node.keys = full_node.keys[:t - 1]

        if not full_node.leaf:
            new_node.children = full_node.children[t:]
            full_node.children = full_node.children[:t]

    def insert(self, key):
        root = self.root
        if len(root.keys) == 2 * self.t - 1:
            new_root = BTreeNode(self.t, False)
            new_root.children.append(self.root)
            self._split_child(new_root, 0, root)
            self.root = new_root

        self._insert_non_full(self.root, key)

    def _insert_non_full(self, node, key):
        if node.leaf:
            node.keys.append(key)
            node.keys.sort()
        else:
            i = len(node.keys) - 1
            while i >= 0 and key < node.keys[i]:
                i -= 1
            i += 1
            if len(node.children[i].keys) == 2 * self.t - 1:
                self._split_child(node, i, node.children[i])
                if key > node.keys[i]:
                    i += 1
            self._insert_non_full(node.children[i], key)

class BPlusTree(BTree):
    def __init__(self, t):
        super().__init__(t)

    def _insert_non_full(self, node, key):
        if node.leaf:
            node.keys.append(key)
            node.keys.sort()
        else:
            i = len(node.keys) - 1
            while i >= 0 and key < node.keys[i]:
                i -= 1
            i += 1
            if len(node.children[i].keys) == 2 * self.t - 1:
                self._split_child(node, i, node.children[i])
                if key > node.keys[i]:
                    i += 1
            self._insert_non_full(node.children[i], key)

    def range_query(self, start, end):
        current = self.root
        while not current.leaf:
            current = current.children[0]
        results = []
        while current:
            for key in current.keys:
                if start <= key <= end:
                    results.append(key)
            current = current.children[-1] if current.children else None
        return results
```
# Complexity
1. **Insertion**: $O(\log(n))$.
2. **Search**: $O(\log(n))$.
3. **Deletion**: $O(\log(n))$.
4. **Range Queries (B+ Tree)**: $O(k + \log(n))$, where $k$ is the number of results.
---
1. **Storage**: $O(n)$.
2. **Auxiliary Space**: $O(h)$, where $h = \log(n)$.