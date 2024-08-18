# Optimization Problem 
Break problems  down into overlapping subproblems, storing their solutions to avoid redundant calculations and achieve optimal efficiency.

# Theory
Divide problems into smaller subproblems, stores their results, and build up the smaller solution to the main problem, leveraging both the overlapping subproblems and optimal substructure properties.

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
$$O(n)$$
DP avoids redundant calculations by storing subproblem results, leading to a linear time complexity. Space complexity is also $O(n)$ due to the storage array used.