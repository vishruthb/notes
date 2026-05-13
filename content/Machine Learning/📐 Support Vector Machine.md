if the data is linearly separable, there are infinitely many valid separators. the issue is that a [[👓 Perceptron|👓 perceptron]] just stumbles onto whichever one its update path leads to, which isn't optimal.
# support vectors
the training points that sit right on the margin boundary, or the points where $y^i(w*x^i + b) = 1$ exactly. these are the points important for defining the boundary. the solution takes the form:

$$w=\sum_{i=1}^{n}\alpha_iy^{(i)}x^{(i)}$$

where $\alpha_i > 0$ only for support vectors and $\alpha_i=0$ for all other points (they contribute nothing).
# hard-margin svm
among all possible separating hyperplanes, hard-margin svm picks the one with maximum margin, or the one farthest from the nearest training point on either side.

for hyperplane $w*x+b=0$, the distance from any point $x$ to this hyperplane is $\frac{|w*x+b|}{||w||}$. for any separating hyperplane we're able to rescale $w$ and $b$ such that the closest points satisfy $y^i(w*x^i + b) = 1$ exactly (support vectors). under this scaling, the margin becomes $\frac{1}{||w||}$. thus, maximizing the margin means maximizing $\frac{1}{||w||}$ or minimizing $||w||$ (or equivalently, $||w||^2$).

formally put, the optimization problem becomes a matter of $\min_{w, b}||w||^2$ subject to $y^i(w*x^i + b) \geq 1$ for all $i=1,...,n$, or in other words, every single training point is on the correct side of the boundary with a margin of at least 1.
# soft-margin svm
unfortunately, hard-margin svm works only on linearly separable data, and in most cases, data is almost never perfectly separable. be it noise, outliers, overlapping classes, etc. soft-margin svm allows us to define some tolerance for mistakes via a **slack variable** $\xi_i \geq 0$ for each training point.

the core optimization problem can be formalized as:

$$\min_{w, b, \boldsymbol{\xi}} \|w\|^2 + C \sum_{i=1}^{n} \xi_i \qquad \text{s.t.} \quad y^{(i)}(w \cdot \mathbf{x}^{(i)} + b) \geq 1 - \xi_i, \quad \xi_i \geq 0 \;\; \forall\, i$$

the slack $\xi_i$ relaxes the constraint, but we still pay for it since the objective function includes a penalty $C$, which is the slack penalty weight. having $C$ allows us to be more or less lenient with allowing data points to cross the margin or decision boundary, and it's generally chosen using cross-validation.

| value of $\xi_i$ | implication for $i$                                                           |
| ---------------- | ----------------------------------------------------------------------------- |
| $\xi_i=0$        | point is on/beyond correct side of margin                                     |
| $0<\xi_i<1$      | point has crossed into margin, but still on correct side of decision boundary |
| $\xi_i=1$        | point is exactly on decision boundary ($w*x+b=0$)                             |
| $\xi_i>1$        | point is on the wrong side of the decision boundary – misclassified           |

a point is a support vector in a soft-margin svm if it's either exactly on the margin with $\xi_i = 0$ or it has any positive slack (e.g. any point with $\xi_i > 0$ is a support vector),
# multiclass
similar to multiclass perceptron, where we have one weight vector per class with biasses, and prediction is $\hat{y} = \text{argmax}_j(w_j * x + b_j)$.

for each training point, the score of the correct class must beat the score of every wrong class by at least $1-\xi_i$.

per training point, there are $k-1$ constraints per training point, one for each class that isn't the true label. the total main constraints are $n(k-1)$, plus $n$ nonnegativity constraints $\xi_i \geq 0$.

$k*d$ weight components + $k$ biases + $n$ slack variables.