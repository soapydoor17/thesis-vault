---
tags:
  - definition
---
## Definition
Gradient descent is a method for unconstrained mathematical optimization. It is a first-order iterative algorithm for minimizing a differentiable multivariate function.
The idea is to take repeated steps in the opposite direction of the gradient (or approximate gradient) of the function at the current point, because this is the direction of steepest descent. 

Conversely, stepping in the direction of the gradient will lead to a trajectory that maximizes that function; the procedure is then known as gradient ascent. 
Gradient descent should not be confused with local search algorithms, although both are iterative methods for optimization.

### Steps
- **Step 1** initialize the parameters of the model randomly 
- **Step 2** Compute the gradient of the cost function with respect to each parameter. It involves making partial differentiation of cost function with respect to the parameters. 
	- Cost function: e.g. Mean Square Error $J(w,b)=\frac1n \sum^n_{i=1}​(\hat{y}_i​−y_i)^2$ 
	- Gradient:
		- with respect to $w$: $\frac{\partial J}{\partial w} = \frac2n \sum^n_{i=1} (\hat{y}_i​−y_i) \cdot x$ 
- **Step 3** Update the parameters of the model by taking steps in the opposite direction of the model. Here we choose a [hyperparameter learning rate](https://www.geeksforgeeks.org/machine-learning/hyperparameter-tuning/) which is denoted by $\gamma$. It helps in deciding the step size of the gradient. 
	- $w=w-\gamma \cdot \frac{\partial J}{\partial w}$ 
- **Step 4** Repeat steps 2 and 3 iteratively to get the best parameter for the defined model.

## Source
[Wikipedia](https://en.wikipedia.org/wiki/Gradient_descent)
[GeeksForGeeks](https://www.geeksforgeeks.org/machine-learning/what-is-gradient-descent/)