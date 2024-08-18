# Map Problem
Maintain a set of key-value pairs with quick insertion, removal, and retrieval.
# Theory
Are indexed data structures through **key-value pairs** with quick retrieval, insertion, and deletion. To handle collisions, we can use **chaining** (where each slot holds a list of items that hashed to the same slot) or **open addressing** (where a collision triggers a sequence to find an empty slot).

# Implementation
```python
class HashNode:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.next = None

class HashMap:
    def __init__(self, capacity=10):
        self.capacity = capacity
        self.size = 0
        self.buckets = [None] * self.capacity

    def get_hash(self, key):
        return hash(key) % self.capacity

    def insert(self, key, value):
        index = self.get_hash(key)
        node = self.buckets[index]
        if not node:
            self.buckets[index] = HashNode(key, value)
            self.size += 1
            return
        prev = None
        while node:
            if node.key == key:
                node.value = value
                return
            prev = node
            node = node.next
        prev.next = HashNode(key, value)
        self.size += 1

    def get(self, key):
        index = self.get_hash(key)
        node = self.buckets[index]
        while node:
            if node.key == key:
                return node.value
            node = node.next
        return None

    def remove(self, key):
        index = self.get_hash(key)
        node = self.buckets[index]
        prev = None
        while node:
            if node.key == key:
                if prev:
                    prev.next = node.next
                else:
                    self.buckets[index] = node.next
                self.size -= 1
                return
            prev = node
            node = node.next
```

# Runtime
All operations in the best case are $O(1)$ when there are no collisions, or if the resolution is efficient. 