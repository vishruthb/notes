# counting problem++
determine the number of ways to choose $r$ elements from $n$ without regard for order.
# theory:
the binomial coefficient, often written as $C(n, r)$ or $\binom{n}{r}$, is:

$$
C(n, r) = \frac{n!}{r!(n - r)!}
$$

used in binomial expansions like:

$$
(x + y)^n = \sum_{r=0}^{n} C(n, r) x^{n-r} y^r
$$

where:
- $n$ : total number of objects
- $r$ : number of objects to choose