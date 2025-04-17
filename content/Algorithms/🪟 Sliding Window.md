# Subarray or Substring Problem
Given an array, find the maximum (or minimum) value in each subarray of size $k$ as the subarray slides from the beginning to the end of the array.
# Theory
Maintain data structure (like a deque) to track relevant elements in the current window as it moves across the array.
# Implementation
```python
from collections import deque

def max_sliding_window(nums, k):
    d = deque()
    result = []
    
    def enqueue(i):
        while d and nums[d[-1]] <= nums[i]:
            d.pop()
        d.append(i)
    
    for i in range(len(nums)):
        enqueue(i)
        if d[0] == i - k:
            d.popleft()
        if i >= k - 1:
            result.append(nums[d[0]])
    
    return result
```
# Runtime
Time: $O(n)$
Space: $O(k)$, where $k$ is the size of the sliding window.