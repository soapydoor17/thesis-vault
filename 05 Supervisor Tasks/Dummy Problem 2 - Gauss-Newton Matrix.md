---
Assigned: 2026-09-03
Due: 2026-09-08
Status: Active
Type: dummy-problems
tags:
  - task
---
Gradient Descent equations for correcting initial mean anomaly and semi-major axis from doppler measurements

For the typical quadratic loss gradient descent problem, the matrix is approximately equivalent to the 'Fisher Information Matrix', which shows how well each parameter is observable from the data. The typical way to see it is through eigenvectors/eigen values of the matrix.

Look at observability with a single vs multiple data points.

# Generalised Gauss-Newton Matrix

> [!info] Taken from [1412.1193](https://arxiv.org/pdf/1412.1193)

The curvature matrix $G$ which arise in the Gauss-Newton method for non-linear least squares problems. It is applicable to the standard neural network training objective $h$ in the case where $L(y,z) = \frac12 || y - z || ^2$ and is given by
$$
G = \frac1{|S|} \sum_{(x,y)\in S} J_f^{\top} J_f \; ,
$$
where $J_f$ is the Jacobian of $f(x, \theta)$ w.r.t the parameters $\theta$. It is usually defined as a modified version of the Hessian $H$ of $h$ (w.r.t $\theta$), obtained by dropping the second term inside the sum in the following expression for $H$:
$$
H=\frac1{|S|} \sum_{(x,y)\in S} \Big( J_f^\top J_f - \sum^m_{j=1} [y- f(x,\theta)]_j H_{[f]_j} \Big) \; ,
$$
where $H_{|f|_j}$ is the Hessian (w.r.t $\theta$) of the $j$-th component of $f(x,\theta)$.
- $G=H$ when $y=f(x,\theta)$ 
- If $y$'s are well-describe by the model $f(x,\theta) + \epsilon$ for i.i.d noise $\epsilon$, then $G=H$ will hold approximately

## Notes from Meeting 08-09-26
Eventually do all orbital elements

TLE -> 2 pertubation coefficients

THUS 8 parameters in total

Are they all observable?

Maybe strict GD doesn't work but kalman filter helps