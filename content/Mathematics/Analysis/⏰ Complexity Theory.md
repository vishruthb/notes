## Big-O
- Upper bound (worst-case scenario)
- $f(n)$ is $O(g(n))$ --> $Ag(n) \geq f(n)$ as $n \rightarrow \infty$
## Big-Ω
- Lower bound
- $f(n)$ is $\Omega(g(n))$ --> $Bg(n) \leq f(n)$ as $n \rightarrow \infty$
## Big-Θ
- Both upper and lower bound
- $f(n)$ is $\Theta(g(n))$ if $f(n)$ is $O(g(n))$ AND $f(n)$ is $\Omega(g(n))$ --> $B(g(n) \leq f(n) \leq Ag(n)$

# Finding Big-O 
1) Determine $f(n)$, the # of operations vs. $n$
2) Drop all lower terms of $n$
3) Drop constant coefficients