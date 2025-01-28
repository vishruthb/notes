# Self-Balancing Binary Search Tree Problem
Efficiently maintain a balanced binary search tree with guaranteed $O(\log(n))$ time complexity for insertion, deletion, and search operations. Red-Black Trees are useful in scenarios where balanced tree structures are critical, such as implementing associative containers like maps and sets.
# Theory
A Red-Black Tree is a self-balancing Binary Search Tree (BST) that enforces additional constraints to ensure balance:
- **Every node is either red or black.**
- **The root node is always black.**
- **Red nodes cannot have red children (no two consecutive red nodes).**
- **Every path from a node to its descendant null pointers must have the same number of black nodes (black height).**

The balance is maintained during insertion and deletion operations via **color flips** and **rotations** (left and right).
# Implementation
```python
class Node:
    def __init__(self, key):
        self.key = key
        self.color = "RED"  # New nodes are always red
        self.left = None
        self.right = None
        self.parent = None

class RedBlackTree:
    def __init__(self):
        self.TNULL = Node(0)  # Sentinel node representing null leaves
        self.TNULL.color = "BLACK"
        self.root = self.TNULL

    def _rotate_left(self, x):
        y = x.right
        x.right = y.left
        if y.left != self.TNULL:
            y.left.parent = x
        y.parent = x.parent
        if x.parent is None:
            self.root = y
        elif x == x.parent.left:
            x.parent.left = y
        else:
            x.parent.right = y
        y.left = x
        x.parent = y

    def _rotate_right(self, x):
        y = x.left
        x.left = y.right
        if y.right != self.TNULL:
            y.right.parent = x
        y.parent = x.parent
        if x.parent is None:
            self.root = y
        elif x == x.parent.right:
            x.parent.right = y
        else:
            x.parent.left = y
        y.right = x
        x.parent = y

    def _fix_insert(self, k):
        while k.parent.color == "RED":
            if k.parent == k.parent.parent.right:
                u = k.parent.parent.left  # Uncle node
                if u.color == "RED":
                    u.color = "BLACK"
                    k.parent.color = "BLACK"
                    k.parent.parent.color = "RED"
                    k = k.parent.parent
                else:
                    if k == k.parent.left:
                        k = k.parent
                        self._rotate_right(k)
                    k.parent.color = "BLACK"
                    k.parent.parent.color = "RED"
                    self._rotate_left(k.parent.parent)
            else:
                u = k.parent.parent.right  # Uncle node
                if u.color == "RED":
                    u.color = "BLACK"
                    k.parent.color = "BLACK"
                    k.parent.parent.color = "RED"
                    k = k.parent.parent
                else:
                    if k == k.parent.right:
                        k = k.parent
                        self._rotate_left(k)
                    k.parent.color = "BLACK"
                    k.parent.parent.color = "RED"
                    self._rotate_right(k.parent.parent)
            if k == self.root:
                break
        self.root.color = "BLACK"

    def insert(self, key):
        node = Node(key)
        node.parent = None
        node.left = self.TNULL
        node.right = self.TNULL
        node.color = "RED"

        y = None
        x = self.root

        while x != self.TNULL:
            y = x
            if node.key < x.key:
                x = x.left
            else:
                x = x.right

        node.parent = y
        if y is None:
            self.root = node
        elif node.key < y.key:
            y.left = node
        else:
            y.right = node

        if node.parent is None:
            node.color = "BLACK"
            return

        if node.parent.parent is None:
            return

        self._fix_insert(node)
```
# Complexity
1. **Insertion**: $O(\log(n))$.
2. **Search**: $O(\log(n))$.
3. **Deletion**: $O(\log(n))$.

The balancing operations (color flips and rotations) guarantee logarithmic height for all operations.

1. **Storage**: $O(n)$ to store all nodes.
2. **Auxiliary Space**: $O(h)$ recursion depth during operations, where $h = \log(n)$.