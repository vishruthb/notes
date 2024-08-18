# Pairs Problem
Find pairs in a sorted array whose sum equals a given value $x$. This technique is effective and commonly used for problems where the array is sorted.

# Theory 
Use two pointers at the array's ends; adjust them inward based on whether their sum is less than or greater than $x$, until the pair is found or the pointers cross.

# Implementation
```python
def is_pair_sum(A, X):
    i, j = 0, len(A) - 1
    while i < j:
        if A[i] + A[j] == X:
            return True
        elif A[i] + A[j] < X:
            i += 1
        else:
            j -= 1
    return False
```

# Runtime
Time: $O(n)$
Space: $O(1)$