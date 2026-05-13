# partitioning problem
determine the number of ways to partition indistinguishable objects into distinguishable bins.

# theory:
the number of ways to distribute $n$ indistinguishable objects into $k$ distinguishable bins is given by:

$$
C(n + k - 1, k - 1) = \frac{(n + k - 1)!}{(k - 1)! n!}
$$

where:
- $n$ : number of indistinguishable objects
- $k$ : number of distinguishable bins

if each bin must contain at least one object, the formula becomes:

$$
C(n - 1, k - 1)
$$