The perceptron is the simplest algorithm for finding a linear classifier, and its usable when data is linearly separable.

At a high level, the algorithm looks something like:
1. Initialize $w=0$ and $b=0$
2. Perform forward pass through training data. For each training point $(x^i, y^i)$:
	(a) Compute the functional margin via $y^i(w*x^i + b)$.
	(b) If ≤ 0, the point is misclassified and we perform an update: $w \leftarrow w + y^ix^i$, $b \leftarrow b + y^i$
	(c) If > 0, the point is correctly classified. Nothing further is done.
3. We repeat until no mistakes are made on a full pass, or we never converge if that doesn't happen.
# Convergence Theorem
This theorem tells us how quickly the perceptron converges when the data is separable. It's stated in terms of two key quantities:
- $R = \max_i ||x^i||$, the radius of the data/norm of farthest point from origin
- $\gamma$, the margin or how easily separable the data is
Assuming there exists a unit vector $w^*$ with $||w^*||=1$ such that every training point satisfies $y^i(w*x^i + b) \geq \gamma$ for some $\gamma>0$, then **the number of mistakes that the perceptron makes is $\leq \frac{R^2}{\gamma^2}$**. Uniform scaling doesn't change this bound. Adding a single new point with a very large norm can increase $R$ without changing $\gamma$, which does increase the bound.

# Multiclass
Going beyond binary labels $\{-1, +1\}$, multiclass perceptrons deal with $k$ classes $\{1, 2, ..., k\}$. Instead of just one weight vector, we have one weight vector **per class** $w_1,...w_k$ and biases $b_1,...,b_k$. Each class $j$ has a score for a point $x$, calculated via $\text{score}_j(x) = w_j * x + b_j$ .

To predict, we just pick the class with the highest score, formalized as:
$$\hat{y} = \text{argmax}_j(w_j*x+b_j)$$

When a point $(x, y)$ with true label $y$ is misclassified as $\hat{y}$, we boost the correct class via:
- $w_y \leftarrow w_y + x$
- $b_y \leftarrow b_y + 1$
And penalize the wrong prediction:
- $w_\hat{y} \leftarrow w_\hat{y} - x$
- $b_\hat{y} \leftarrow b_\hat{y} - 1$
While keeping all the other $k-2$ weight vectors unchanged.