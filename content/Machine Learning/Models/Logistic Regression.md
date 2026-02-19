Squashing function:

$$s(z) = \frac{1}{1+e^{-z}}$$
Given some data $(x^1, y^1), ...,(x^n,y^n) \in R^d \times \{-1, 1\}$ , our goal is to minimize the loss function $L(w, b)$:

$$L(w, b) = -\sum_{i=1}^{n} \ln \text{Pr}_{w,b}(y^{(i)} \mid x^{(i)}) = \sum_{i=1}^{n} \ln(1 + e^{-y^{(i)}(w \cdot x^{(i)} + b)})$$
Issue is that there is no closed-form solution for $w$, but fortunately, $L(w)$ is convex in $w$. We can use **local search** to find the minimum of this function, via **gradient descent**.