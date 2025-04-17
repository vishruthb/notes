# Complete Search Problem
Explore all possible solutions to a problem, such as generating all subsets or permutations, by recursively searching the entire solution space.
# Theory
 Break down a problem into smaller instances of itself, solving each by calling the function within itself until a base case is met.
# Implementation
```python
def generate_subsets(arr, index=0, current=[]):
    if index == len(arr):
        print(current)
        return
    generate_subsets(arr, index + 1, current + [arr[index]])
    generate_subsets(arr, index + 1, current)
```
# Runtime
Time Complexity: $O(2^n)$
Space Complexity: $O(n)$

Each element has two choices: to be included or not, leading to an exponential number of subsets. The space complexity is also linear due to the call stack holding $n$ frames during the recursion.