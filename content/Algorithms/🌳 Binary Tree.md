# Tree Traversal
Navigate through all the nodes in a binary tree, typically to perform some operation on each node.

# Theory
A binary tree is a hierarchical structure consisting of nodes, each having up to two children referred to as the left and right child. It's used for various operations, like sorting and searching data, due to its hierarchical nature.

# Implementation

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

def inorder_traversal(root):
    if root:
        inorder_traversal(root.left)
        print(root.value)
        inorder_traversal(root.right)

def preorder_traversal(root):
    if root:
        print(root.value)
        preorder_traversal(root.left)
        preorder_traversal(root.right)

def postorder_traversal(root):
    if root:
        postorder_traversal(root.left)
        postorder_traversal(root.right)
        print(root.value)
```

# Runtime
**Traversal**: $O(n)$ for inorder, preorder, and postorder traversals, since each node is visited exactly once.