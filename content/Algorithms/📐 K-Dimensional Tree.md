# Multidimensional Search Problem
Efficiently search, insert, and store data points in a k-dimensional space, such as points in a 2D or 3D plane. K-D Trees are commonly used in range search, nearest neighbor search, creating point clouds, etc.
# Theory
A K-Dimensional (K-D) Tree is a binary search tree extended to k-dimensional space. Each level of the tree partitions the space along a specific axis:

- At depth `d`, the axis used for comparison is `d % k`, where `k` is the number of dimensions.
- Nodes store points in k-dimensional space, and left/right subtrees represent points on either side of the split axis.

The K-D Tree is particularly useful in applications where space-partitioning and efficient point queries (e.g., nearest neighbor) are required.
# Implementation
```python
class KDTree:
    def __init__(self, points=None, k=2):
        self.k = k
        self.root = self._build_tree(points, depth=0) if points else None

    class Node:
        def __init__(self, point, left=None, right=None):
            self.point = point
            self.left = left
            self.right = right

    def _build_tree(self, points, depth):
        if not points:
            return None

        axis = depth % self.k
        points.sort(key=lambda x: x[axis])
        median = len(points) // 2

        return self.Node(
            point=points[median],
            left=self._build_tree(points[:median], depth + 1),
            right=self._build_tree(points[median + 1:], depth + 1)
        )

    def _distance_squared(self, point1, point2):
        return sum((x - y) ** 2 for x, y in zip(point1, point2))

    def nearest_neighbor(self, target, node=None, depth=0, best=None):
        if node is None:
            node = self.root

        if node is None:
            return best

        axis = depth % self.k
        next_best = best
        next_branch = None

        if best is None or self._distance_squared(target, node.point) < self._distance_squared(target, best):
            next_best = node.point

        if target[axis] < node.point[axis]:
            next_branch = node.left
            other_branch = node.right
        else:
            next_branch = node.right
            other_branch = node.left

        next_best = self.nearest_neighbor(target, next_branch, depth + 1, next_best)

        if (target[axis] - node.point[axis]) ** 2 < self._distance_squared(target, next_best):
            next_best = self.nearest_neighbor(target, other_branch, depth + 1, next_best)

        return next_best
```
# Complexity
1. **Insertion**: $O(\log(n))$ for balanced data.
2. **Search (Nearest Neighbor)**: $O(\log(n))$ for balanced data, but $O(n)$ in the worst case.
3. **Construction**: $O(n \cdot \log(n))$.
---
1. **Storage**: $O(n)$ to store all nodes.
2. **Auxiliary Space**: $O(h)$ recursion depth during queries or construction, where $h = \log(n)$ for balanced data.
