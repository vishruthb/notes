# theory
objects are arranged linearly. unlike an array, order in a linked list is determined by a pointer in each object. elements contain keys that can be searched for, providing simple, flexible representation for dynamic sets.

a **singly linked list** only has a `next` pointer. a **doubly linked list** has elements with an attribute `key` and two pointer attributes: `next` and `prev`. in **circular** list, the `prev` pointer of the head points to the `tail`, and the `next`  pointer of the tail points to the head.
# implementation
```python
class ListNode:
    def __init__(self, key, next=None, prev=None):
        self.key = key
        self.next = next
        self.prev = prev

class LinkedList:
    def __init__(self):
        self.head = None

    def search(self, key):
        x = self.head
        while x is not None and x.key != key:
            x = x.next
        return x

    def prepend(self, key): # insert at beginning
        new_node = ListNode(key, next=self.head)
        if self.head is not None:
            self.head.prev = new_node
        self.head = new_node

    def insert(self, x, key):
        new_node = ListNode(key, next=x.next, prev=x)
        if x.next is not None:
            x.next.prev = new_node
        x.next = new_node

    def delete(self, x):
        if x.prev is not None:
            x.prev.next = x.next
        else:
            self.head = x.next
        if x.next is not None:
            x.next.prev = x.prev
```
# runtime
- `search`: $O(n)$ in the worst case, since it may have to search the entire list.
- `prepend`: $O(1)$ on a list of $n$ elements.
- `insert`, `delete`: $O(1)$ for a doubly linked list, at beginning or the end.