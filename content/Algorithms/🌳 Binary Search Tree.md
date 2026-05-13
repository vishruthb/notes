# storing data
a binary search tree (bst) is a hierarchical data structure that stores elements in a way that allows for efficient insertion, deletion, and searching operations. each node in a bst satisfies the following property:

- **bst property**: for any node `N`:
  - all nodes in the left subtree of `N` have values **less than** `N`'s value.
  - all nodes in the right subtree of `N` have values **greater than** `N`'s value.
### traversals
bsts support the same traversal methods as binary trees:
- **inorder traversal**: produces a sorted order of elements in the bst.
- **preorder traversal**: processes the root before its subtrees (useful for copying trees).
- **postorder traversal**: processes subtrees before the root (useful for deleting trees).
# implementation
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
# complexity
1. **insertion**: $O(h)$, where $h$ is the height of the tree.
2. **search**: $O(h)$.
3. **deletion**: $O(h)$.

- in the best case (balanced tree), $h = \log(n)$, so these operations are $O(\log(n))$.
- in the worst case (skewed tree), $h = n$, so these operations are $O(n)$.

1. **storage**: $O(n)$ to store all the nodes.
2. **auxiliary space**: $O(h)$ in recursion during operations like traversal.