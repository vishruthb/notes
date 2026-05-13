the perceptron is the simplest algorithm for finding a linear classifier, and its usable when data is linearly separable.

at a high level, the algorithm looks something like:
1. initialize $w=0$ and $b=0$
2. perform forward pass through training data. for each training point $(x^i, y^i)$:
	- compute the functional margin via $y^i(w*x^i + b)$
	- if ≤ 0, the point is misclassified and we perform an update: $w \leftarrow w + y^ix^i$, $b \leftarrow b + y^i$
	- if > 0, the point is correctly classified. nothing further is done
3. we repeat until no mistakes are made on a full pass, or we never converge if that doesn't happen.
# convergence theorem
this theorem tells us how quickly the perceptron converges when the data is separable. it's stated in terms of two key quantities:
- $R = \max_i ||x^i||$, the radius of the data/norm of farthest point from origin
- $\gamma$, the margin or how easily separable the data is

assuming there exists a unit vector $w^*$ with $||w^*||=1$ such that every training point satisfies $y^i(w*x^i + b) \geq \gamma$ for some $\gamma>0$, then **the number of mistakes that the perceptron makes is $\leq \frac{R^2}{\gamma^2}$**. uniform scaling doesn't change this bound. adding a single new point with a very large norm can increase $R$ without changing $\gamma$, which does increase the bound.
# multiclass
going beyond binary labels $\{-1, +1\}$, multiclass perceptrons deal with $k$ classes $\{1, 2, ..., k\}$. instead of just one weight vector, we have one weight vector **per class** $w_1,...w_k$ and biases $b_1,...,b_k$. each class $j$ has a score for a point $x$, calculated via $\text{score}_j(x) = w_j * x + b_j$ .

to predict, we just pick the class with the highest score, formalized as:

$$\hat{y} = \text{argmax}_j(w_j*x+b_j)$$

when a point $(x, y)$ with true label $y$ is misclassified as $\hat{y}$, we boost the correct class via:
- $w_y \leftarrow w_y + x$
- $b_y \leftarrow b_y + 1$

and penalize the wrong prediction:
- $w_\hat{y} \leftarrow w_\hat{y} - x$
- $b_\hat{y} \leftarrow b_\hat{y} - 1$

while keeping all the other $k-2$ weight vectors unchanged.