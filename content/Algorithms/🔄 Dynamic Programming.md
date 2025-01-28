# Optimization Problem 
Break problems down into overlapping subproblems, storing their solutions (memoization) to avoid redundant calculations and achieve optimal efficiency as opposed to brute force recursion.

# Implementation
```python
def fibonacci(n):
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]
    return dp[n]
```

# Runtime
DP avoids redundant calculations by storing subproblem results, leading to a  time complexity of $O(n)$. Space complexity is also $O(n)$ due to the storage array used.