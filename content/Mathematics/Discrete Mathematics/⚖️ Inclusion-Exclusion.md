# set counting problem
calculate the union of multiple sets while avoiding overcounting.

# theory:
to count the union of two sets $A$ and $B$, the inclusion-exclusion principle is applied as:

$$
|A \cup B| = |A| + |B| - |A \cap B|
$$

for three sets, the formula is:

$$
|A \cup B \cup C| = |A| + |B| + |C| - |A \cap B| - |B \cap C| - |A \cap C| + |A \cap B \cap C|
$$

this formula ensures that no elements are counted multiple times.
