# minimum value problem
maintain a set that support quick insertions and minimum-value retrievals.
# theory
a queue works as **first-in, first-out (fifo)**.

the `insert` operation for a stack is often called `enqueue`, and `delete` operation is usually `dequeue`. has a **head** and a **tail**, with new elements going at the tail-end of the queue, and removed elements coming from the head.
# implementation
we use `dequeue` from python's `collections` module.

```python
from collections import deque

q = deque()
q.append(1) # append = enqueue
q.popleft() # popleft = dequeue
```
# runtime
all above operations are in $O(1)$ time. we can also implement a queue using lists to get a $O(n)$ time complexity.
