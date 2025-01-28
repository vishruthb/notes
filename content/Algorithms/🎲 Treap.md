# Prioritized Search Problem
Maintain a binary search tree order and also prioritize nodes based on an independent priority, making it useful for tasks like order-statistics, range queries, and scenarios that involve randomization for balancing.
# Theory
A Treap is a randomized data structure that combines the properties of a Binary Search Tree (BST) and a Heap:
- **BST Property**: Nodes are ordered by their keys.
- **Heap Property**: Nodes are prioritized by their associated priorities.

In addition to maintaining the BST ordering of keys, a Treap ensures that the priority of every node is greater than or equal to the priorities of its children.

Treaps achieve balance through randomized insertion and rotation operations, maintaining an expected $O(\log(n))$ height.
# Implementation
```python
import random

class Node:
    def __init__(self, key, priority=None):
        self.key = key
        self.priority = priority if priority else random.randint(1, 100)
        self.left = None
        self.right = None

class Treap:
    def __init__(self):
        self.root = None

    def _rotate_right(self, node):
        new_root = node.left
        node.left = new_root.right
        new_root.right = node
        return new_root

    def _rotate_left(self, node):
        new_root = node.right
        node.right = new_root.left
        new_root.left = node
        return new_root

    def _insert(self, node, key, priority):
        if not node:
            return Node(key, priority)

        if key < node.key:
            node.left = self._insert(node.left, key, priority)
            if node.left.priority > node.priority:
                node = self._rotate_right(node)
        elif key > node.key:
            node.right = self._insert(node.right, key, priority)
            if node.right.priority > node.priority:
                node = self._rotate_left(node)
        return node

    def insert(self, key, priority=None):
        self.root = self._insert(self.root, key, priority)

    def inorder_traversal(self, node):
        if node:
            self.inorder_traversal(node.left)
            print(f"({node.key}, {node.priority})", end=" ")
            self.inorder_traversal(node.right)
```
# Complexity
1. **Insertion**: $O(\log(n))$ (expected due to randomization).
2. **Search**: $O(\log(n))$.
3. **Deletion**: $O(\log(n))$.

The randomized nature ensures balance in expectation, leading to logarithmic height.

1. **Storage**: $O(n)$ to store all nodes.
2. **Auxiliary Space**: $O(h)$ recursion depth, where $h = \log(n)$ in expectation.
