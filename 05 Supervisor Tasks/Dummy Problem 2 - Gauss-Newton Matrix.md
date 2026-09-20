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

### GD vs G-NM
Gradient Descent looks at which direction decreases the loss in that exact moment
Gauss-Newton Matrix (and Fisher Info Matrix) looks at how sharply the loss changes in different directions of parameter space, or whether some combinations of parameters are nearly interchangeable as far as the data is concerned
- how do parameters relate across all observations

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

# Method

## 1. Define orbital model
Model how satelite's position and velocity depend on orbital parameters

COE2RV - from Chapter 2.6 of Fundations of Astrodynamics and Applications by D. Vallado

```python
def COE2RV(coe, mu=3.986004418*(10**14)):
    # INPUT: 
    #   coe is an array of the Keplerian Orbital Elements
    #       a - semi-major axis (m)
    #       e - eccentricity
    #       i - inclination (deg)
    #       node - right ascension of the ascending node (deg)
    #       arg - argument of perigee (deg)
    #       nu - true anomaly (radians)
    #   mu - gravitational parameters (=GM). Default set to the value for Earth
	
    # OUTPUT:
    #   r - position vector of satellite
    #   v - velocity vector of satellite
	
	
    a, e, i, node, arg, nu = coe
	
    sin_nu = np.sin(nu)
    cos_nu = np.cos(nu)
	
    # Calculate semiparameter (p)
    p = a * (1-e**2)
	
    # Perifocal Coordinate System
    R_PQW = np.zeros(3)
    V_PQW = np.zeros(3)
	
    R_PQW[0] = (p * cos_nu) / (1 + e*cos_nu)
    R_PQW[1] = (p * sin_nu) / (1 + e*cos_nu)
    
    V_PQW[0] = -1 * np.sqrt(mu/p) * sin_nu
    V_PQW[1] = np.sqrt(mu/p) * (e + cos_nu)
	
    # Rotation Matrix
    R_node_z = R.from_euler('z', np.deg2rad(node)).as_matrix()
    R_i_x    = R.from_euler('x', np.deg2rad(i)).as_matrix()
    R_arg_z  = R.from_euler('z', np.deg2rad(arg)).as_matrix()
    R_total  = R_node_z @ R_i_x @ R_arg_z
	
    # Rotate Perifocal to IJK
    R_IJK = R_total @ R_PQW
    V_IJK = R_total @ V_PQW
	
    return R_IJK, V_IJK
```

### Maths
Need semiparameter $p$ instead of $a$:
$$
p = a(1-e^2)
$$
Need to calculate $\nu$ from $M_0$:
- $n$ is the mean motion
$$
M(t) = M_0 + n (t-t_0)
$$
$$
n = \sqrt{\frac{\mu}{a^3}}
$$
- Use Newton-Raphson Method (Algorithm 2 in FoAaA pg. 65) to find $E$
	- Converts $M=E-e\sin (E)$ to be in terms of $E$
- The calculate $\nu$
$$
\cos(\nu) = \frac{\cos(E) - e}{1-e\cos(E)}
$$
```python
def n(a, mu=3.986004418*(10**14)):
    # Returns the mean motion in rads/s
    return np.sqrt(mu / a**3)
```

```python
def NewtRaph(M, e, tolerance=10**-8):
    # Converts single pair of M and e to E
    # Inputs:
    #   M - Mean anomaly (degrees)
    #   e - Eccentricity
    #   tolerance - default to 10^8
    # Outputs:
    #   E - Eccentric anomaly
    
    M_rad = np.deg2rad(M)
	
    if (M_rad>-np.pi and M_rad<0) or (M_rad>np.pi):
        E = M_rad-e
    else:
        E = M_rad+e
	
    while True:
        sin_E = np.sin(E)
        cos_E = np.cos(E)
        nextE = E + (M_rad-E+e*sin_E)/(1-e*cos_E)
		
        abs_diff = abs(nextE-E)
        E = nextE
		
        if abs_diff < tolerance:
            break
	
    return E
```

```python
def MultiNewtRaph(t, M_0, n, e, tolerance=10**-8):
    # Inputs:
    #   t - array of time entries (s)
    #   M_0 - initial mean anomaly (deg)
    #   n - mean motion (rads/s) 
    #   e - eccentricity
    # Outputs:
    #   nu_t - true anomaly (in radians) at each point in time
	
    t_0 = t[0]
	
    # Mean anomalies
    M_t = np.zeros_like(t)
    M_t = M_0 + n*(t-t_0)
	
    # Newton-Raphson to find eccentric anomaly (E)
    E_t = np.zeros_like(M_t)
    for i in range(len(M_t)):
        E_t[i] = NewtRaph(M[i], e, tolerance=tolerance)
	
    return E_t
```

```python
def nu(E, e):
    # Inputs:
    #   E - Eccentric anomaly (radians)
    #   e - Eccentricity
    # Outputs:
    #   nu - True Anomaly (radians)
     
    cosE = np.cos(E)
    nu = np.arccos( (cosE - e)/(1 - e*cosE) ) # in radians
    return nu
```

## 2. Get Doppler Shift from $\textbf{r}$ and $\textbf{v}$
Now knowing $\textbf{r}$ and $\textbf{v}$, can solve for $f_D$, as long as we also have ground station vectors ECI
[[ECI vs ECEF]]

### Getting Ground Station Vectors
Position $\textbf{r}_{gs}$
1. Convert geodetic coordinates to (lat/lon/att) to ECEF
2. Rotate ECEF to ECI using Earth's rotation angle at each timestamp (via sidereal time)

Velocity $\textbf{v}_{gs}$
1. Differentiate rotation (angular velocity crossed with position)
### Doppler Shift Equation
INPUT:
- $\textbf{r}$ - position vector of satellite relative to the center of the Earth 
	- as calculated from COE2RV
- $\textbf{v}$ - velocity vector of satellite relative to the center of the Earth
	- as calculated from COE2RV
- $\textbf{r}_{gs}$ - position vector of ground station relative to the center of the Earth
	- HOW???
- $\textbf{v}_{gs}$ - velocity vector of ground station relative to the center of the Earth
	- HOW???
- $f_c$ - Beacon center frequency 
	- from given data
- $c$ - Speed of light = $299792458 \ m/s$

OUTPUT
- $f_D$ - Doppler shift
	- To be the prediction for the model

```python
def f_D(r, v, r_gs, v_gs, f_c, c=299792458):
    rho = r - r_gs
    rho_hat = rho / np.sqrt(rho.dot(rho))
	
    v_rel = v - v_gs
	
    k = f_c/c
	
    f_D = k * rho_hat * (-1 * v_rel)
	
    return f_D
```

## 3. Find the error/residual

$$
\epsilon = \Delta f_D= f_{D \ {pred}} - f_{D \ {true}}
$$

```python
def residual(y_pred, y_true):
    return y_pred-y_true
```

## 4. Construct Jacobian
[Resource](https://www.geeksforgeeks.org/data-science/jacobian-and-hessian-matrices/) on Jacobian and Hessian Matrices

Jacobian matrix is $N \times P$ where $N$ is the number of observations and $P$ is the number of parameters

Let $\boldsymbol{\theta} = (M_0, a)$ 

### Single Observation
$$ J(\boldsymbol \theta)=
\frac{\partial \epsilon}{\partial \boldsymbol \theta} = 
\begin{bmatrix}
\dfrac{\partial \epsilon}{\partial M_0} & \dfrac{\partial \epsilon}{\partial a} \\
\end{bmatrix}
$$

### Multiple Observations
Where $n$ is the number of observations
$$ J (\boldsymbol \theta)=
\dfrac{\partial \boldsymbol \epsilon}{\partial \boldsymbol{\theta}} = 
\begin{bmatrix}
\dfrac{\partial \epsilon_{1}}{\partial M_0} & \dfrac{\partial \epsilon_{1}}{\partial a} \\
\dfrac{\partial \epsilon_{2}}{\partial M_0} & \dfrac{\partial \epsilon_{2}}{\partial a} \\
\vdots & \vdots \\
\dfrac{\partial \epsilon_{n}}{\partial M_0} & \dfrac{\partial \epsilon_{n}}{\partial a} \\
\end{bmatrix}
$$
### Partial Derivatives
Relationship between Doppler shift and satellite cartesian state:
- where $I$ is an identity matrix (3x3 for x,y,z dimensions)
$$
\frac{ \partial f_{D} }{ \partial \textbf X } = \left[ -k \ v_{rel}^{T} \left( \frac{I}{||\rho||} - \frac{\rho\rho^{T}}{||\rho||^3} \right), \ -k\hat\rho^T \right]
$$
Derivative of the cartesian state of the satellite:
$$
\frac{d \textbf X}{dt} = \left[ v, - \frac{\mu r}{||r||^{3}} \right]^T \\
$$

```python
def dfD_dXCalc(k, v_rel, rho, rho_hat):
    # Relationship between Doppler shift and satellite's cartesian state
	
    mag_rho = np.sqrt(rho.dot(rho))
	
    I3 = np.eye(3)
	
    dfD_dr = -k * v_rel.T @ ( I3/mag_rho - (np.outer(rho, rho))/(mag_rho**3) )
    dfD_dv = -k * rho_hat.T
    return np.concatenate([dfD_dr, dfD_dv])


def dX_dtCalc(v, r, mu=MU_EARTH):
    # Derivative of the satellite's cartesian state
	
    mag_r = np.sqrt(r.dot(r))
    dv_dt = -mu * r / (mag_r**3)
	
    return np.concatenate([v, dv_dt])
```

Initial Mean Anomaly Gradient:
- $\frac{ \partial f_{D} }{ \partial \textbf X }$ is a 1x6 matrix
- $\frac{dX}{dt}$ is a 6x1 matrix
- $\frac{ \partial M }{ \partial t }$ will be a constant for each individual time
- Thus gradient is a single value for each time
$$
\begin{gathered}
\frac{ \partial f_{D} }{ \partial M_{0} } = \frac{ \partial f_{D} }{ \partial \textbf X } \frac{d \textbf X}{dt} \left( \frac{dM}{dt} \right)^{-1}\frac{ \partial M }{\partial M_{0}} \\
\left( \frac{dM}{dt} \right)^{-1}= \frac{T_{p}}{360} = \frac{\pi}{180n} \\
\frac{ \partial M }{ \partial M_{0} } = 1
\end{gathered}
$$

```python
def dfD_dM_0Calc(dfD_dX, dX_dt, n):
    # Initial Mean Anomaly Gradient
    # Inputs:
    #   dfD_dX  - Change in Doppler measurement w.r.t. satellite's cartesian state (1x6 array)
    #   dX_dt   - Change in satellite's cartesian state w.r.t. time (6x1 array)
    #   n      - Mean motion (rad/s)
    # Outputs:
    #   dfD_dM_0 - Change in Doppler shift w.r.t. Initial Mean Anomaly (Hz/deg)
	
    dM_dt_reciprocal = 1 / np.rad2deg(n)
	
    return dfD_dX @ dX_dt * dM_dt_reciprocal
```

Semi-Major Axis Gradient:
$$
\begin{gathered}

\frac{ \partial f_{D} }{ \partial a } = \frac{ \partial f_{D} }{ \partial \textbf X } \frac{d\textbf{X}}{da} \\

\frac{d\textbf X}{da} = \left. \frac{ \partial \textbf X }{ \partial a } \right|_{M} + \frac{ \partial \textbf X }{ \partial M } \frac{ \partial M }{ \partial a } \\

\left. \frac{ \partial \textbf X }{ \partial a } \right|_{M} = \begin{bmatrix}
R(1-e\cos E) \begin{bmatrix}\cos \nu \\ \sin \nu \\ 0\end{bmatrix}, \quad
-\dfrac{n}{2-2e\cos E} \ R \begin{bmatrix}-\sin E \\ \sqrt{ 1-e^{2} } \, \cos E \\ 0\end{bmatrix}
\end{bmatrix}^T\\

\dfrac{ \partial \textbf X }{ \partial M } = \frac{1}{n} \frac{dX}{dt} \\

\dfrac{ \partial M }{ \partial a } = -\frac{3}{2} \sqrt{ \frac{\mu}{a^5} } \ t

\end{gathered}
$$
```python
def dfD_daCalc(dfD_dX, dX_dt, t, rot_mat, n, a, e, E, nu, mu=MU_EARTH):
    # Semi-Major Axis Gradient
    # Inputs:
    #   dfD_dX  - Change in Doppler measurement w.r.t. satellite's cartesian state (1x6 array)
    #   dX_dt   - Change in satellite's cartesian state w.r.t. time (6x1 array)
    #   t       - Time of observation (s)
    #   rot_mat - Perifocal-to-ECI rotation matrix (from COE2RV)
    #   n       - Mean motion (rad/s)
    #   a       - Semi-major axis (m)
    #   e       - Eccentricity
    #   E       - Eccentric anomaly (rad)
    #   nu      - True anomaly (rad)
    #   mu      - Gravitational parameter (m^3/s^2) (Defaults to Earth's mu value)
    # Outputs:
    #   dfD_da  - Change in Doppler shift w.r.t semi-major axis
    # NOTE: For calculations in this function, M is in radians
	
    nu_arr = np.array([np.cos(nu), np.sin(nu), 0])
    E_arr = np.array([-np.sin(E), np.sqrt(1-e**2) * np.cos(E), 0])
	
    dr_da_M = (1 - e*np.cos(E)) * rot_mat @ nu_arr
    dv_da_M = -n / (2- 2*e*np.cos(E)) * rot_mat @ E_arr
	
    dX_da_M = np.concatenate([dr_da_M, dv_da_M])
    dX_dM = dX_dt / n
    dM_da = -3/2 * np.sqrt(mu / (a**5)) * t
	
    dX_da = dX_da_M + dX_dM * dM_da
	
    return dfD_dX @ dX_da    
```

## 5. Stack the Jacobian rows
For each row in input data, construct Jacobian row.

```python
def Jacobian(dfD_dM_0, dfD_da):
    # Create Jacobian Matrix
    # Inputs:
    #   dfD_dM_0 - Initial mean anomaly gradient (scalar or Nx1 array)
    #   dfD_da   - Semi-major axis gradient (scalar or Nx1 array)
    # Outputs:
    #   J - Outputs Nx2 array, where N is the number of observations 
	
    # Convert scalar values to 1D arrays
    dfD_dM_0 = np.atleast_1d(dfD_dM_0)
	dfD_da = np.atleast_1d(dfD_da)
	
    N = len(dfD_dM_0)       # Number of entries
	
    # Create matrix
    J = np.zeros((N, 2))
    for i in range(N):
        J[i] = np.array([dfD_dM_0[i], dfD_da[i]])
	
    return J
```

## 6. Form the Gauss-Newton Matrix
$$
G= \frac{1}{N} J^TJ
$$
Will be a 2x2 matrix (two parameters)
- Diagonal parameters -> how sensitive residuals are to each parameter individually. 
- Off-diagonal entry -> whether changing $a$ and changing $M_0$ produce _similar-looking effects_ on your residuals

```python
def GaussNewton(J):
    # Create Gauss-Newton Matrix
    # Inputs:
    #   J - Nx2 Jacobian Matrix, where N is the number of observations
    # Outputs:
    #   G - 2x2 Gauss-Newton Matrix
     
    N = J.shape[0]
    return 1/N * J.T @ J
```

## 7. Compute eigenvalues and eigenvectors of $G$

**NOT IMPLEMENTED YET**

Function from number gets

```python
eig_w, eig_v = np.linalg.eig(G)
```

# Results

## Single Observation

## Multiple Observations

# Discussion
- Any eigenvalues zero or near-zero?
- What directions do eigenvectors point in?
- What does direction mean physically? (Is a parameter invisible to measurement?)
Compare the two case
- How does smallest eigenvalue change across cases while adding data points?
- Do degenerate directions become better constrained or worse?
Interpret
- Which parameter combinations are well-observed vs poorly
- Why does it make sense given Doppler measurements and what they can/can't distinguish?