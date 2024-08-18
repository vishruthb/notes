# Theory
Used to get the smallest element (min heap) whenever the element is popped. 

## Types
- **MaxHeap**: The key present at the root node must be the **maximum** of all it's children.
- **MinHeap**: Key present at the root node must be the **minimum** among all of it's child keys.

> [!Note]
 > Same properties wil be true for all subtrees in that [[🌳 Binary Tree]]

# Implementation
We use the `heapq` module in python.

```python
import heapq

sample = [4, 5, 7, 2, 3]

heapq.heapify(sample) # [2, 3, 4, 5, 7]

heapq.heappush(sample, 6) # [2, 3, 4, 5, 6, 7]

heapq.heapppop(sample) # Pops 2 >> smallest element
```

# Runtime
Insertion and deletion (of the smallest element) is in $O(\log(n))$.