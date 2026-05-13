# probability distribution problem
calculate the probability of a certain number of successes in independent trials.

# theory:
- **bernoulli trial**: a random experiment with two possible outcomes: success (with probability $p$) and failure (with probability $1 - p$).

- **binomial distribution**: the probability of exactly $k$ successes in $n$ independent bernoulli trials is given by:

$$
P(X = k) = C(n, k) p^k (1 - p)^{n - k}
$$

where:
- $n$: number of trials
- $k$: number of successes
- $p$: probability of success in each trial