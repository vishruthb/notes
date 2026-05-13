# theory:
closed-form expression:
$$F_n = \frac{\phi^n - (-\phi)^{-n}}{\sqrt{5}}$$
# tabulation
bottom-up
```python
def fib_dp(n):
    if n <= 1:
        return 1
    # Initialize a table to store Fibonacci values
    fib = [0] * (n + 1)
    fib[0], fib[1] = 1, 1

    # Fill the table iteratively
    for i in range(2, n + 1):
        fib[i] = fib[i - 1] + fib[i - 2]

    return fib[n]
```

# memoization
cached recursion
```python
def fib_dp_memo(n, memo={}):
    if n <= 1:
        return 1
    if n not in memo:
        # Store the result in memo for reuse
        memo[n] = fib_dp_memo(n - 1, memo) + fib_dp_memo(n - 2, memo)
    return memo[n]
```

# runtime

| approach    | time   | space  |
| ----------- | ------ | ------ |
| tabulation  | $O(n)$ | $O(n)$ |
| memoization | $O(n)$ | $O(1)$ |

