## Theory

A stack works as **last-in, first-out (LIFO)**.

The `insert` operation for a stack is often called `push`, and `delete` operation is usually `pop`. An attempt to pop an empty stack results in an **underflow**, and if the top index value exceeds the size of the stack, we get an **overflow**.

## Implementation

```python
# using a singly linked list

class Node:
	def __init__(self, value):
		self.value = value
		self.next = None
		
class Stack:
	def __init__(self):
		self.head = Node("head")
		self.size = 0
		
	def __str__(self):
		cur = self.head.next
		out = ""
		while cur:
			out += str(cur.value) + "->"
			cur = cur.next
		return out[:-2]
		
	def getSize(self):
		return self.size

	def isEmpty(self):
		return self.size == 0

	def top(self):
		return self.head.next.value

	def push(self, value):
		node = Node(value)
		node.next = self.head
		self.head = node
		self.size += 1

	def pop(self):
		remove = self.head.next
		self.head.next = self.head.next.next
		self.size -= 1
		return remove.value
```

## Runtime

All above operations are in $O(1)$ time.

## Further Notes

- **Advantage:** Efficient for adding/removing elements. Can be used to reverse the order of elements, implement undo/redo functions, etc.
- **Drawback:** Does not provide fast access to elements other than the top element, not efficient for searching (need to pop one by one until specific element is found).