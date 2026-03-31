# Map Problem
Maintain a set of key-value pairs with quick insertion, removal, and retrieval.
# Theory
Are data structures indexed through **key-value pairs** with quick retrieval, insertion, and deletion. To handle collisions, we can use **chaining** (where each slot holds a list of items that hashed to the same slot) or **open addressing** (where a collision triggers a sequence to find an empty slot).
# Implementation
```python
class Node:
    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.next = None

class HashTable:
    
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.size = 0
        self.table = [None] * self.capacity

    def hash_function(self, key: int) -> int:
        return key % self.capacity

    def insert(self, key: int, value: int) -> None:
        index = self.hash_function(key)
        node = self.table[index]

        if not node:
            self.table[index] = Node(key, value)
        else:
            prev = None
            while node:
                if node.key == key:
                    node.value = value
                    return
                prev, node = node, node.next
            prev.next = Node(key, value)
        
        self.size += 1

        if self.size / self.capacity >= 0.5:
            self.resize()

    def get(self, key: int) -> int:
        index = self.hash_function(key)
        node = self.table[index]

        while node:
            if node.key == key:
                return node.value
            node = node.next

        return -1


    def remove(self, key: int) -> bool:
        index = self.hash_function(key)
        node = self.table[index]
        prev = None

        while node:
            if node.key == key:
                if prev:
                    prev.next = node.next
                else:
                    self.table[index] = node.next
                self.size -= 1
                return True
            prev, node = node, node.next
        
        return False

    def getSize(self) -> int:
        return self.size

    def getCapacity(self) -> int:
        return self.capacity

    def resize(self) -> None:
        self.capacity *= 2
        new_table = [None] * self.capacity

        for node in self.table:
            while node:
                index = node.key % self.capacity
                if new_table[index] is None:
                    new_table[index] = Node(node.key, node.value)
                else:
                    new_node = new_table[index]
                    while new_node.next:
                        new_node = new_node.next
                    new_node.next = Node(node.key, node.value)
                node = node.next
        
        self.table = new_table
```
# Runtime
All operations in the best case are $O(1)$ when there are no collisions, or if the resolution is efficient. 