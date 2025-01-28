# Balancing Without Rotations Problem
Efficiently maintain a balanced binary search tree by leveraging random priorities, eliminating the need for explicit balancing operations (like rotations). This is particularly useful when balancing logic must be minimal and randomized distribution suffices.
# Theory
A Randomized Search Tree (RST) is a probabilistic data structure that combines properties of a Binary Search Tree (BST) and a random priority heap:
- **BST Property**: Nodes are ordered by their keys.
- **Heap Property**: Nodes are prioritized by their randomly assigned priorities.

The key difference between RSTs and Treaps is that RSTs use the priority to construct the tree **without requiring rotations**. This leads to a simpler implementation, but the expected runtime guarantees are the same.

RSTs maintain balance probabilistically, achieving expected logarithmic height by inserting nodes in a manner that respects both the BST and heap properties.
# Implementation
```python
import random

class Node:
    def __init__(self, key, priority=None):
        self.key = key
        self.priority = priority if priority else random.randint(1, 100)
        self.left = None
        self.right = None

class RST:
    def __init__(self):
        self.root = None

    def _split(self, node, key):
        if not node:
            return None, None
        if key < node.key:
            left, node.left = self._split(node.left, key)
            return left, node
        else:
            node.right, right = self._split(node.right, key)
            return node, right

    def _merge(self, left, right):
        if not left or not right:
            return left if left else right
        if left.priority > right.priority:
            left.right = self._merge(left.right, right)
            return left
        else:
            right.left = self._merge(left, right.left)
            return right

    def insert(self, key, priority=None):
        new_node = Node(key, priority)
        if not self.root:
            self.root = new_node
        else:
            left, right = self._split(self.root, key)
            self.root = self._merge(self._merge(left, new_node), right)

    def inorder_traversal(self, node):
        if node:
            self.inorder_traversal(node.left)
            print(f"({node.key}, {node.priority})", end=" ")
            self.inorder_traversal(node.right)
```
# Complexity
1. **Insertion**: $O(\log(n))$ (expected).
2. **Search**: $O(\log(n))$ (expected).
3. **Deletion**: $O(\log(n))$ (expected).

The randomized structure ensures expected logarithmic height without the overhead of rotation logic.

1. **Storage**: $O(n)$ to store all nodes.
2. **Auxiliary Space**: $O(h)$ recursion depth, where $h = \log(n)$ in expectation.