# Counting problem++
Determine the number of ways to choose $r$ elements from $n$ without regard for order.
# Theory:
The binomial coefficient, often written as $C(n, r)$ or $\binom{n}{r}$, is:

$$
C(n, r) = \frac{n!}{r!(n - r)!}
$$

Used in binomial expansions like:

$$
(x + y)^n = \sum_{r=0}^{n} C(n, r) x^{n-r} y^r
$$

Where:
- $n$ : Total number of objects
- $r$ : Number of objects to choose