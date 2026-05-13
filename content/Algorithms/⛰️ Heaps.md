# theory
used to get the smallest element (min heap) whenever the element is popped. in a min-heap, for a node at index $i$, the left child is at index $2 \times i$, the right child is at index $2 \times i + 1$, and the parent is at index $i//2$.

## types
- **maxheap**: the key present at the root node must be the **maximum** of all it's children.
- **minheap**: key present at the root node must be the **minimum** among all of it's child keys.

> [!note]
 > similar properties will be true for all subtrees in [[🌳 Binary Search Tree|🌳 binary search tree]], where each parent node has a value less than or equal to its children's values. thus, we're able to always access the smallest element at the root of the heap.

# implementation
we can use the `heapq` module in python.

```python
import heapq

sample = [4, 5, 7, 2, 3]

heapq.heapify(sample) # [2, 3, 4, 5, 7]
heapq.heappush(sample, 6) # [2, 3, 4, 5, 6, 7]
heapq.heappop(sample) # pops and returns 2, the smallest element
```

alternatively, we can design our own minimum heap (or priority queue) class.

```python
class MinHeap:
	def __init__(self):
		self,heap = [0]

	def push(self, val: int) -> None:
		self.heap.append(val)
		i = len(self.heap) - 1 # setting to index of new value being appended

		# now we percolate up
		while i > 1 and self.heap[i] < self.heap[i // 2]:
			self.heap[i], self.heap[i // 2] = self.heap[i  // 2], seelf.heap[i] # swap val with parent to maintain min-heap property
			i = i // 2

	def pop(self) -> int:
		if len(self.heap) <= 1:
			return -1
		if len(self.heap) == 2:
			return self.heap.pop()

	    res = self.heap[1]
        # move last value to root
        self.heap[1] = self.heap.pop()
        i = 1
        # percolate down
        while 2 * i < len(self.heap):
            smallest_child = 2 * i
            if 2 * i + 1 < len(self.heap) and self.heap[2 * i + 1] < self.heap[2 * i]:
                smallest_child = 2 * i + 1
            if self.heap[i] > self.heap[smallest_child]:
                self.heap[i], self.heap[smallest_child] = self.heap[smallest_child], self.heap[i]
                i = smallest_child
            else:
                break
        return res

    def top(self) -> int:
        if len(self.heap) > 1:
            return self.heap[1]
        return -1

    def heapify(self, arr: List[int]) -> None:
        self.heap = [0] + arr  # start heap at index 1
        n = len(self.heap) - 1
        for i in range(n // 2, 0, -1):
            self._percolate_down(i)

    def _percolate_down(self, i: int) -> None:
        while 2 * i < len(self.heap):
            smallest_child = 2 * i
            if 2 * i + 1 < len(self.heap) and self.heap[2 * i + 1] < self.heap[2 * i]:
                smallest_child = 2 * i + 1
            if self.heap[i] > self.heap[smallest_child]:
                self.heap[i], self.heap[smallest_child] = self.heap[smallest_child], self.heap[i]
                i = smallest_child
            else:
                break

```
# runtime
insertion and deletion (of the smallest element) is in $O(\log(n))$.