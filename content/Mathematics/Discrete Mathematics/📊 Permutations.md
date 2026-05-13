# counting and arrangement problem
determine the number of ways to arrange objects where the order matters.

# theory:
the number of ways to arrange $r$ objects from a set of $n$ distinct objects is given by:

$$
P(n, r) = \frac{n!}{(n - r)!}
$$

where:
- $n$ : total number of distinct objects
- $r$ : number of objects to arrange

# multisets (permutations with repetition):
if some elements are repeated, the number of distinct permutations is given by:

$$
\frac{n!}{k_1! k_2! \dots k_m!}
$$

where $k_1, k_2, \dots, k_m$ represent the frequencies of the repeated elements.