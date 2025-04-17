# Partitioning problem++
Determine the number of non-negative integer solutions to an equation.

# Theory:
The number of non-negative integer solutions to the equation:

$$
a_1 + a_2 + \dots + a_k = n
$$

is given by:

$$
C(n + k - 1, k - 1) = \frac{(n + k - 1)!}{(k - 1)! n!}
$$

Where:
- $n$ : Total sum
- $k$ : Number of variables
