# Storing Data
A Binary Search Tree (BST) is a hierarchical data structure that stores elements in a way that allows for efficient insertion, deletion, and searching operations. Each node in a BST satisfies the following property:

- **BST Property**: For any node `N`:
  - All nodes in the left subtree of `N` have values **less than** `N`'s value.
  - All nodes in the right subtree of `N` have values **greater than** `N`'s value.
### Traversals
BSTs support the same traversal methods as binary trees:
- **Inorder Traversal**: Produces a sorted order of elements in the BST.
- **Preorder Traversal**: Processes the root before its subtrees (useful for copying trees).
- **Postorder Traversal**: Processes subtrees before the root (useful for deleting trees).
# Implementation
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

class BinarySearchTree:
    def __init__(self):
        self.root = None

    def insert(self, value):
        if not self.root:
            self.root = Node(value)
        else:
            self._insert(self.root, value)

    def _insert(self, current, value):
        if value < current.value:
            if current.left is None:
                current.left = Node(value)
            else:
                self._insert(current.left, value)
        elif value > current.value:
            if current.right is None:
                current.right = Node(value)
            else:
                self._insert(current.right, value)

    def inorder_traversal(self, root):
        if root:
            self.inorder_traversal(root.left)
            print(root.value, end=" ")
            self.inorder_traversal(root.right)

    def preorder_traversal(self, root):
        if root:
            print(root.value, end=" ")
            self.preorder_traversal(root.left)
            self.preorder_traversal(root.right)

    def postorder_traversal(self, root):
        if root:
            self.postorder_traversal(root.left)
            self.postorder_traversal(root.right)
            print(root.value, end=" ")
```
# Complexity
1. **Insertion**: $O(h)$, where $h$ is the height of the tree.
2. **Search**: $O(h)$.
3. **Deletion**: $O(h)$.

- In the best case (balanced tree), $h = \log(n)$, so these operations are $O(\log(n))$.
- In the worst case (skewed tree), $h = n$, so these operations are $O(n)$.

1. **Storage**: $O(n)$ to store all the nodes.
2. **Auxiliary space**: $O(h)$ in recursion during operations like traversal.