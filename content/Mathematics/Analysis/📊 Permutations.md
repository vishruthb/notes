# Counting and arrangement problem
Determine the number of ways to arrange objects where the order matters.

# Theory:
The number of ways to arrange $r$ objects from a set of $n$ distinct objects is given by:

$$
P(n, r) = \frac{n!}{(n - r)!}
$$

Where:
- $n$ : Total number of distinct objects
- $r$ : Number of objects to arrange

# Multisets (Permutations with Repetition):
If some elements are repeated, the number of distinct permutations is given by:

$$
\frac{n!}{k_1! k_2! \dots k_m!}
$$

Where $k_1, k_2, \dots, k_m$ represent the frequencies of the repeated elements.