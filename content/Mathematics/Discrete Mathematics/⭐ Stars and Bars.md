# Partitioning problem
Determine the number of ways to partition indistinguishable objects into distinguishable bins.

# Theory:
The number of ways to distribute $n$ indistinguishable objects into $k$ distinguishable bins is given by:

$$
C(n + k - 1, k - 1) = \frac{(n + k - 1)!}{(k - 1)! n!}
$$

Where:
- $n$ : Number of indistinguishable objects
- $k$ : Number of distinguishable bins

If each bin must contain at least one object, the formula becomes:

$$
C(n - 1, k - 1)
$$