# Range Sum Problem
Given a fixed 1D array, efficiently compute the sum of elements between any two indices for multiple queries.

# Theory
Preprocess the array into a cumulative sum array, allowing for constant-time range sum queries by subtracting the cumulative sum at the start index from the cumulative sum at the end index.

# Implementation
```python
def prefix_sums(arr):
    prefix = [0]
    for num in arr:
        prefix.append(prefix[-1] + num)
    return prefix

N, Q = map(int, input().split())
arr = list(map(int, input().split()))
prefix = prefix_sums(arr)
for _ in range(Q):
    l, r = map(int, input().split())
    print(prefix[r] - prefix[l])
```

# Runtime
$$O(N+Q)$$
Prefix sums are computed in linear time, and each range sum query is handled in constant time, making the overall complexity efficient for large inputs. Space complexity is $O(N)$ because an additional array of size $N + 1$ is used to store the prefix sums.