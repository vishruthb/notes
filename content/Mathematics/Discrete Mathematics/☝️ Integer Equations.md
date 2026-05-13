# partitioning problem++
determine the number of non-negative integer solutions to an equation.

# theory:
the number of non-negative integer solutions to the equation:

$$
a_1 + a_2 + \dots + a_k = n
$$

is given by:

$$
C(n + k - 1, k - 1) = \frac{(n + k - 1)!}{(k - 1)! n!}
$$

where:
- $n$ : total sum
- $k$ : number of variables
