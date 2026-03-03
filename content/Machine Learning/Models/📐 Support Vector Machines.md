Soft-margin SVM:
$$\min_{w,b,\xi} \|w\|^2 + C \sum_{i=1}^{n} \xi_i$$
The hyperparameter $C$ is the slack penalty weight, which allows us to be more or less lenient with allowing data points to cross the margin or decision boundary.