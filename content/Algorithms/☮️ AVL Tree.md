# Balancing Problem
An AVL Tree is a self-balancing Binary Search Tree (BST). It maintains the following property:

- **Balance Factor**: For every node, the height of the left and right subtrees differs by at most 1.

To maintain this property, AVL Trees perform rotations (single or double) whenever an insertion or deletion causes a node to violate the balance factor constraint.
### Rotations
1. **Left Rotation (LL)**: Performed when the left subtree is taller by 2 and the imbalance comes from the left child's left subtree.
2. **Right Rotation (RR)**: Performed when the right subtree is taller by 2 and the imbalance comes from the right child's right subtree.
3. **Left-Right Rotation (LR)**: A combination of left and right rotations to fix imbalance from the left child's right subtree.
4. **Right-Left Rotation (RL)**: A combination of right and left rotations to fix imbalance from the right child's left subtree.
# Implementation
```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None
        self.height = 1

class AVLTree:
    def __init__(self):
        self.root = None

    def _get_height(self, node):
        return node.height if node else 0

    def _get_balance(self, node):
        return self._get_height(node.right) - self._get_height(node.left) if node else 0

    def _rotate_left(self, z):
        y = z.right
        T2 = y.left

        # Perform rotation
        y.left = z
        z.right = T2

        # Update heights
        z.height = 1 + max(self._get_height(z.left), self._get_height(z.right))
        y.height = 1 + max(self._get_height(y.left), self._get_height(y.right))

        return y

    def _rotate_right(self, z):
        y = z.left
        T3 = y.right

        # Perform rotation
        y.right = z
        z.left = T3

        # Update heights
        z.height = 1 + max(self._get_height(z.left), self._get_height(z.right))
        y.height = 1 + max(self._get_height(y.left), self._get_height(y.right))

        return y

    def insert(self, node, value):
        if not node:
            return Node(value)

        if value < node.value:
            node.left = self.insert(node.left, value)
        elif value > node.value:
            node.right = self.insert(node.right, value)
        else:
            return node

        # Update height of the current node
        node.height = 1 + max(self._get_height(node.left), self._get_height(node.right))

        # Get the balance factor
        balance = self._get_balance(node)

        # Balance the node if it is unbalanced
        if balance < -1 and value < node.left.value:
            return self._rotate_right(node)
        if balance > 1 and value > node.right.value:
            return self._rotate_left(node)
        if balance < -1 and value > node.left.value:
            node.left = self._rotate_left(node.left)
            return self._rotate_right(node)
        if balance > 1 and value < node.right.value:
            node.right = self._rotate_right(node.right)
            return self._rotate_left(node)

        return node

    def inorder_traversal(self, node):
        if node:
            self.inorder_traversal(node.left)
            print(node.value, end=" ")
            self.inorder_traversal(node.right)
```
# Complexity
1. **Insertion**: $O(\log(n))$.
2. **Search**: $O(\log(n))$.
3. **Deletion**: $O(\log(n))$.

The balancing operations (rotations) ensure that the height of the AVL Tree remains $O(\log(n))$, guaranteeing efficient operations.

1. **Storage**: $O(n)$ to store all the nodes.
2. **Auxiliary space**: $O(h)$ (height of the tree) for recursion during operations.
