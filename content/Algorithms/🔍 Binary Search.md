# Search Problem
Given a sorted array, efficiently find the index of a target value or determine if it is present in the array.
# Theory
Recursively divide the search interval in half, comparing the target value to the middle element of the array and narrowing down the search to the left or right half until the target is found or the interval is empty.
# Implementation
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```
# Runtime
$$O(n\log{n})$$
With each iteration, binary search reduces the search space by half, leading to a logarithmic time complexity. Space complexity is $\text{O}(1)$. 