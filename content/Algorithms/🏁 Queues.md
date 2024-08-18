# Minimum Value Problem
Maintain a set that support quick insertions and minimum-value retrievals.
# Theory
A queue works as **first-in, first-out (FIFO)**.

The `insert` operation for a stack is often called `enqueue`, and `delete` operation is usually `dequeue`. Has a **head** and a **tail**, with new elements going at the tail-end of the queue, and removed elements coming from the head.

# Implementation
We use `dequeue` from Python's `collections` module.

```python
from collections import deque

q = deque()
q.append(1) # append = enqueue
q.popleft() # popleft = dequeue
```

# Runtime
All above operations are in $O(1)$ time. We can also implement a queue using lists to get a $O(n)$ time complexity.
