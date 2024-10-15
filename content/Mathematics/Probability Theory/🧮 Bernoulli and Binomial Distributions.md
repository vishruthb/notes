# Probability distribution problem
Calculate the probability of a certain number of successes in independent trials.

# Theory:
- **Bernoulli Trial**: A random experiment with two possible outcomes: success (with probability $p$) and failure (with probability $1 - p$).
  
- **Binomial Distribution**: The probability of exactly $k$ successes in $n$ independent Bernoulli trials is given by:

$$
P(X = k) = C(n, k) p^k (1 - p)^{n - k}
$$

Where:
- $n$: Number of trials
- $k$: Number of successes
- $p$: Probability of success in each trial