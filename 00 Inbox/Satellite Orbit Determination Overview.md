#infodump
# Video 1 - Main Background
Video: https://www.youtube.com/watch?v=2xmPfzeiJyk
## Background
GOAL:  Generate TLE from optical observations (angle only)
[[Two-Line Element (TLE)]]

## Initial Orbit Determination
Two or three observations
- Two allows for circular orbit
- Three more accurate estimate to account for eccentricity 
This is hopefully sufficient to recover the object in a short time-span (to recover on same night + additional observations)

## Differential Correction
Allows refining of initial orbit
Multi-dimensional Newton-Raphson approach

---
### Newton-Raphson
Can be used to find the square root of two, by finding the root of:
$$
f(x)=x^2 -2
$$
#### Formula:
$$
x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}
$$
In this case:
- $f(x)=x^2 -2$
- $f'(x)=2x$

Iteration 1: $x_1 = 1- \frac{1^2-2}{2\cdot 1} = 1.5$
Iteration 2: $x_2=1.4167$
Iteration 3: $x_3=1.4142$
$\rightarrow$ quickly converges to $\sqrt{2} \approx 1.4142$ 

---

Differential correction is similar but with multiple elements to solve for

> [!info] Main Idea
> Minimise the residuals between the **observed** and **computed** values of $alpha$, $delta$ across all observations. i.e:
> $$ \overrightarrow{r} = \overrightarrow{y}_{\text{obs}} - \overrightarrow{y}_{\text{model}}(\overrightarrow{X})$$
> Where $\overrightarrow{y}_{\text{model}} (\overrightarrow{X})$ is computed by:
> 1. Converting $\overrightarrow{X}$ into initial position/velocity at epoch
> 2. Propagating to each $t_i$
> 3. Coverting $\overrightarrow{r}(t_i)$ to topocentric RA/Dec for the observer location

$$
\overrightarrow{X} = [a \quad e \quad i \quad \Omega \quad \omega \quad M_0]^T
$$
Thus $\overrightarrow{X}$ are the [[Keplerian (Classical) Orbital Elements]]:
- $a$ - semi-major axis
	- NOTE: this is NOT in the TLE, instead we have Mean Motion
- $e$ - eccentricity
- $i$ - inclination
- $\Omega$ - right ascension of ascending node
- $\omega$ - argument of perigee
- $M_0$ - mean anomaly at epoch

NOTE: Don't typically like the classical elements because of singularities
- e.g. if inclination is 0, the the right ascension of the ascending node is not well defined
- e.g. if eccentricity is 0, then argument of perigee is not well defined

Thus want to transform Classical Elements to Equinoetial Elements

### DC Workflow
Start with initial orbit
Then loop through each observation
Each with their own timestamp -> predict where the satellite would be with the current model (do this for each observation)
Compute residuals (see above)
Solve Jacobian -> partial derivatives
Solve Least Squares
Has it converged yet?
- Epsilon $\epsilon$ determines the tolerance

## Proper Spacing of Observations
https://amostech.com//TechnicalPapers/2012/Astrodynamics/DER.pdf
Don't want observations to be too close or too far apart

# Video 2 - Algorithms
Videos: https://www.youtube.com/watch?v=_kIw2yDNpqQ&list=PLdRvCnMJ1LSEtuZ2Czu_6ueym56fSu62R
Handout: https://sioslab.com/wp-content/uploads/2022/05/4-Orbit-Determination-handout-1.pdf

Brief history on how orbit determination used to be a 'fitting' problem (fitting orbits of naturally occuring bodies) -> Kepler's Laws (based on fits to observational data). 
- Relies heavily on range azimuth and elevation measurements
- As well as separation distance estimates, provided by radar and lidar 
	- Thus only possible if you can get a radar/lidar return from the object (typically can't for more distant bodies and naturally occuring bodies and bodies outside of the solar system)

Video considers cases where range cannot be calculated --> angle only measurements
- The case for when using optical sensors and deep space objects
## Gauss's Method
Also see https://amostech.com//TechnicalPapers/2012/Astrodynamics/DER.pdf

The history behind it is kind of cool! In the 19th century, someone's body law of planetary separation had predicted that there is a planet between the orbit's of Mars and Jupiter. On Jan 1st 1800, Italian astronomer discovered the planetoid Ceres (they didn't know about planetoids yet as the Asteroid Belt hadn't been discovered yet). However, they couldn't find the orbit, there was a cash prize for it and everything and then Gauss came along and solved it, end ultimately invents a whole new world of mathemtical and numerical tools that we still use today.

> [!info] Definition
> The determination of an orbit from three angle-only measurements.

![file:///home/tobey/Downloads/CropHere-com.jpg](file:///home/tobey/Downloads/CropHere-com.jpg)

Consider three position vectors belonging to the same orbit at different times ($\textbf{r}_{P/G} (t_1)$, $\textbf{r}_{P/G} (t_2)$, $\textbf{r}_{P/G} (t_3)$). 

These points are taken from a surface point on the Earth. There is a vector for the position of the object with respect to the observer location ($\textbf{r}_{P/O} (t_x)$), and the vector from the observer to the center of the Earth ($\textbf{r}_{O/G} (t_x)$), thus is it easy to get the full vector 
$$\textbf{r}_{P/G} (t_x) = \textbf{r}_{P/O} (t_x) + \textbf{r}_{O/G} (t_x)$$
From now the position vector of the object with respect to the center of the Earth will be referred to as $\textbf{r}_i$ and that of the object with respect to the observer will be referred to as $\mathbf{\rho}_i$.
NOTE: $\overset{\Delta}{=}$ means 'equivalent by definition'
$$
\begin{align}
\textbf{r}_i \overset{\Delta}{=} \textbf{r}_{P/G} (t_i) \\
\mathbf{\rho}_i \overset{\Delta}{=} \textbf{r}_{P/O} (t_i)
\end{align}
$$
Assuming that this is a two-body orbit, all three vectors must lie within the same plane (the **parafocal frame of orbit**), making the vectors linearly dependent:
$$
c_1 \textbf{r}_1 + c_2 \textbf{r}_2 + c_3 \textbf{r}_3 =0
$$
Can be confirmed as $\textbf{r}_1 \cdot (\textbf{r}_2 \times \textbf{r}_3) = 0$ as  $\textbf{r}_2 \times \textbf{r}_3$ wil give a plane orthongal to the orbital plane, so the dot product with $\textbf{r}_1$ should give 0 as they should be perpendicular.

Assuming $c_2 \neq 0 \rightarrow (c_1\textbf{r}_1 + c_2 \textbf{r}_2 + c_3 \textbf{r}_3) \times \textbf{r}_3 = c_1\textbf{r}_1 \times \textbf{r}_3 + c_2 \textbf{r}_2 \times \textbf{r}_3 = 0 \Rightarrow c_1\textbf{r}_1 \times \textbf{r}_3 = - c_2 \textbf{r}_2 \times \textbf{r}_3$
Therefore:
$$
\begin{align}
c_1 (\textbf{r}_1 \times \textbf{r}_3) = -c_2(\textbf{r}_2 \times \textbf{r}_3) \\
c_3 (\textbf{r}_1 \times \textbf{r}_3) = -c_2(\textbf{r}_1 \times \textbf{r}_2)
\end{align}
$$
Can express any orbital radius vector using $f$ and $g$ functions as a function of another orbital radius vector and velocity vector of the same orbit. Previously thought of $f$ and $g$ functions as propagating the orbit forward in time, but orbits are fully time reversible, so there is no reason why can can't use $f$ and $g$ functions to go back in time. Thus can always represent the position at times $t_1$ and $t_3$ as functions of the position and velocity at time $t_2$:
$$\begin{align}
\textbf{r}_1 = f_1 \textbf{r}_2 + g_1 \textbf{v}_2 \\
\textbf{r}_3 = f_3 \textbf{r}_2 + g_3 \textbf{v}_2
\end{align}$$
Setting $c_2 \overset{\Delta}{=} -1$ means only $c_1$ and $c_3$ need to be scaled.
Determining $c_1$:
$$
\begin{align}
c_1 &= -c_2 \frac{\textbf{r}_2 \times \textbf{r}_3}{\textbf{r}_1 \times \textbf{r}_3} 
= \frac{\textbf{r}_2 \times \textbf{r}_3}{\textbf{r}_1 \times \textbf{r}_3} 
= \frac{\textbf{r}_2 \times (f_3 \textbf{r}_2 + g_3\textbf{v}_2)}{(f_1 \textbf{r}_2 + g_1\textbf{v}_2) \times (f_3 \textbf{r}_2 + g_3\textbf{v}_2)}
= \frac{g_3(\textbf{r}_2 \times \textbf{v}_2)}{f_1 g_3 (\textbf{r}_2 \times \textbf{v}_2) + g_1 f_3 (\textbf{v}_2 \times \textbf{r}_2)} \\
&= \frac{g_3}{f_1 g_3 - f_3 g_1}
\end{align}
$$
Similar can be applied to $c_3$. Therefore:
$$
\begin{align}
c_1 = \frac{g_3}{f_1 g_3 - f_3 g_1} \\
c_3 = \frac{g_1}{f_3 g_1 - f_1 g_3}
\end{align}
$$
---

![[Pasted image 20260909083342.png]]

Define $\sigma$ as the gravitational parameter divided by $r^3$, meaning that the $f$ and $g$ values can calculated in series.
$$
\sigma \overset{\Delta}{=} \frac{\mu}{r^3} \quad \Rightarrow \quad 

\left\{
	\begin{array}{l}
		f = 1 - \frac{\sigma}{2} (\Delta t)^2 \ ... \\
		g = \Delta t - \frac{\sigma}{6} (\Delta t)^3 ...
	\end{array}
\right.
$$
As the intent of this method is preliminary orbit determination and the measurements themselves are intrinsically noisy, there is no need for a whole lot of fidelity in the computation of the $f$ and $g$ values. Just looking for an initial guess to then refine, thus only taking the series to second order (only looking at terms up to $(\Delta t)^2$).
$\Delta t$ is a small time period as the method is targeting small time differences between measurements. Specifically looking at a bunch of successive measurements covering a small arc of the overall orbit.

Thus taking the equation previously established for $c_1$, take all of the $f$ and $g$ functions and replace them with the first few terms of the series solution.
- Follows idea that $f$ and $g$ are mapping $\textbf{r}_1$ and $\textbf{r}_3$ to the center point $\textbf{r}_2$
- Skipping all the working out gets us to:
- $O(\Delta t_i^3)$ is the unused terms from the series I think and don't matter?
$$
\begin{align}
\Delta t_1 \overset{\Delta}{=} t_1-t_2 \quad \quad \Delta t_3 \overset{\Delta}{=} t_3-t_2
\end{align}
$$
$$
\begin{align}
c_1 &= \frac{\Delta t_3}{\Delta t_3 - \Delta t_1} + \frac{\Delta t_3((\Delta t_3-\Delta t_1)^2 - \Delta t^2_3)}{6(\Delta t_3- \Delta t_1)} \sigma + O(\Delta t^3_i) \\
a_1 & \overset{\Delta}= \frac{\Delta t_3}{\Delta t_3 - \Delta t_1} \;, \quad b_1  \overset{\Delta}= \frac{\Delta t_3((\Delta t_3-\Delta t_1)^2 - \Delta t^2_3)}{6(\Delta t_3- \Delta t_1)} \\

\therefore c_1 &= a_1 + b_1 \sigma + O(\Delta t^3_i)
\end{align}
$$
Similarly applied to $c_3$:
$$
\begin{align}
c_3 &= \frac{-\Delta t_1}{\Delta t_3 - \Delta t_1} + \frac{-\Delta t_1((\Delta t_3-\Delta t_1)^2 - \Delta t^2_1)}{6(\Delta t_3- \Delta t_1)} \sigma + O(\Delta t^3_i) \\
a_3 & \overset{\Delta}= \frac{-\Delta t_1}{\Delta t_3 - \Delta t_1} \;, \quad b_3  \overset{\Delta}= \frac{-\Delta t_1((\Delta t_3-\Delta t_1)^2 - \Delta t^2_1)}{6(\Delta t_3- \Delta t_1)} \sigma \\

\therefore c_3 &= a_3 + b_3 \sigma + O(\Delta t^3_i)
\end{align}
$$

---

![[Pasted image 20260909103927.png]]

### Basic Setup
Have three position vectors (or at least their unit vectors) of the object w.r.t the observer at three different times ($\textbf{r}_{P/O}(t_i) \Rightarrow \mathbf{\rho}_i$). Assume the ability to compute the position of the observer w.r.t. the center of mass of the relevant central body (center of the Earth) at the three times ($\textbf{r}_{O/G}(t_i)$). This allows the calculation of the position vector ot the object with respect to the center of mass at three different times ($\textbf{r}_{P/G}(t_i) \Rightarrow \textbf{r}_i$).

Have established linear dependence bteween $\textbf{r}_1$, $\textbf{r}_2$ and $\textbf{r}_3$. This means we can apply the same weightings to $\mathbf{\rho}_i$ and $\textbf{r}_{O/G}(t_i)$:
$$
c_1 \textbf{r}_1 + c_2 \textbf{r}_2 + c_3 \textbf{r}_3 =0 \quad \Rightarrow \quad c_1 \mathbf{\rho}_1 + c_2 \mathbf{\rho}_2 + c_3 \mathbf{\rho}_3 = -(c_1 \textbf{r}_{O/G}(t_1) + c_2 \textbf{r}_{O/G}(t_2) + c_3 \textbf{r}_{O/G}(t_3))
$$
Turn into a linear system by package it as the unit directions of all $\mathbf{\rho}_1$. This becomes the fundament measurement $A$. Measing the angles on the sky at three different times (w.r.t. observer location):
$$
A \overset{\Delta}{=}[\hat{\mathbf{\rho}}_1 \quad \hat{\mathbf{\rho}}_2 \quad \hat{\mathbf{\rho}}_3]
$$
$$
A 
\begin{bmatrix}
c_1 \rho_1 \\
c_2 \rho_2 \\
c_3 \rho_3
\end{bmatrix} 
=
\begin{bmatrix}
\textbf{r}_{O/G}(t_1) & \textbf{r}_{O/G}(t_2) & \textbf{r}_{O/G}(t_3)
\end{bmatrix} 

\begin{bmatrix}
-c_1 \\
-c_2 \\
-c_3
\end{bmatrix} 
$$
As long as $A$ is non-singular, it can be inverted and moved to the right-hand side
$$
\begin{bmatrix}
c_1 \rho_1 \\
c_2 \rho_2 \\
c_3 \rho_3
\end{bmatrix} 
= A^{-1}
\begin{bmatrix}
\textbf{r}_{O/G}(t_1) & \textbf{r}_{O/G}(t_2) & \textbf{r}_{O/G}(t_3)
\end{bmatrix} 

\begin{bmatrix}
-c_1 \\
-c_2 \\
-c_3
\end{bmatrix} 
$$
$$
B \overset{\Delta}=
A^{-1}
\begin{bmatrix}
\textbf{r}_{O/G}(t_1) & \textbf{r}_{O/G}(t_2) & \textbf{r}_{O/G}(t_3)
\end{bmatrix} 
$$
$$ \therefore
\begin{bmatrix}
c_1 \rho_1 \\
c_2 \rho_2 \\
c_3 \rho_3
\end{bmatrix} 
= B 

\begin{bmatrix}
-c_1 \\
-c_2 \\
-c_3
\end{bmatrix} 
$$
Given $c_2 = -1$, as previously discussed, there is a method for calculating $c_1$ and $c_3$. This means we can defined $\rho_2$ as a single row out of matrix $B$. Remember $a_1$, $a_3$, $b_1$ and $b_3$ where defined as coefficients in $c_1$ and $c_3$.
$$
\rho_2 = B_{21} a_1 - B_{22} + B_{23}a_3 + (B_{21} b_1 + B_{23} b_3)\sigma
$$
$$
d_1 \overset{\Delta}{=}B_{21} a_1 - B_{22} + B_{23}a_3 \quad \quad d_2 \overset{\Delta}{=} B_{21} b_1 + B_{23} b_3
$$
$$
\therefore \rho_2 = d_1+d_2\sigma
$$
Remember $r_i = ||\textbf{r}_i|| = ||\mathbf{\rho}_i + \textbf{r}_{O/G}(t_i)||$. Can apply Euclidean norm to get in terms of $\rho_i$, $\mathbf{\rho}_i$ and $\textbf{r}_{O/G}(t_i)$. Apply to $r_2^3$ to get:
$$
r_2^2 = (d_1 + d_2 \sigma)^2 + 2 (d_1+d_2) (\hat{\mathbf{\rho}} \cdot \mathbf{r}_{O/G}(t_2)) + ||\mathbf{r}_{O/G}(t_2)||^2
$$
Thus can apply to $\sigma= \frac{\mu}{r_2^3}$ to get the following eight-order polynomial:
$$
r_2^8 = (d^2_1 + 2 d_1 \hat{\mathbf{\rho}}_2 \cdot \textbf{r}_{O/G}(t_2) + ||\textbf{r}_{O/G}(t_2)||^2) r^6_2 + 2\mu(d_2 \hat{\mathbf{\rho}}_2 \cdot \textbf{r}_{O/G}(t_2) + d_1 d_2) r_2^3 + \mu^2 d^2_2
$$
### Summary
Start with three angle only measurements ($\mathbf{\rho}_i$). Can calculate the unit vectors. Looking for position vectors in order to track the distance the body travels and get a preliminary orbital fit.

Another method uses the position vectors to instantly get the orbital elements of the body, so getting the $\textbf{r}_i$ vectors is equivalent to fitting the orbit.

The Gauss method gives us an approximate approach to finding the $\textbf{r}_i$ given the $\hat{\mathbf{\rho}_i}$ measurements. At the end, we get an 8th order polynomial that gives the distance (geocentric in this case, but in general w.r.t any central body) of the intermediate measurement. The polynamial is made up of the developed coefficients and known quantities (such as offset between observer and the center of mass of the central body). Effectively solve for any positive real root, recalculate sigma value and all $c_i$, matrix $A^{-1}$ and $B$
Eventually get all $\rho_i$

First developed by Gauss to track Ceres, but actually works best when applied to interplanetary trajectors.

Angles between observations should be small (less than 10 degrees), especially because of approximations when finding $c$ coefficients. Better approximations of $f$ and $g$ series give better results, but always limited to first order of $\sigma$ (as derivatives of $r$ required to get derivative of $\sigma$ which are necessary to go to a higher order).

Heaps of literature building and expanding on Gauss.

## Gibb's Method
Going from orbit to orbital elements. Assumes $c_1$, $c_2$ and $c_3$ have all been found, and are linearly dependant.

The dot product of the linear summation with the eccentricity vector will equal zero.
$$
\Big(\sum c_i \textbf{r}_i \Big) \cdot \textbf{e} =0
$$
This leads to the expression (working out in the video):
- NOTE: $\ell$ is the semi-parameter, $\nu$ is the true anomaly
$$
\Big(\sum c_i \textbf{r}_i \Big) \cdot \textbf{e} = c_1 (\ell - r_1) + c_2 (\ell - r_2) + c_3 (\ell - r_3)
$$
Can multiply the expression by the cross product $\textbf{r}_3 \times \textbf{r}_1$ and substitute the linear dependence equations in order to elimate terms, thus:
$$
\ell(\textbf{r}_1 \times \textbf{r}_2 + \textbf{r}_2 \times \textbf{r}_3 + \textbf{r}_3 \times \textbf{r}_1) = r_3(\textbf{r}_1 \times \textbf{r}_2) + r_1(\textbf{r}_2 \times \textbf{r}_3) + r_2 ( \textbf{r}_3 \times \textbf{r}_1)
$$
$$
\textbf{d} \overset{\Delta}{=} (\textbf{r}_1 \times \textbf{r}_2 + \textbf{r}_2 \times \textbf{r}_3 + \textbf{r}_3 \times \textbf{r}_1)
$$
$$
\ell \textbf{d} = \textbf{n} \overset{\Delta}= r_3(\textbf{r}_1 \times \textbf{r}_2) + r_1(\textbf{r}_2 \times \textbf{r}_3) + r_2 ( \textbf{r}_3 \times \textbf{r}_1)
$$
Note that $\textbf{d}$ and $\textbf{n}$ lie in the direction of $\textbf{r}_i \times \textbf{r}_j$. All $\textbf{r}_i$ is coplanar, so this is the direction orthogonal to the orbital plane. This means $\textbf{d}$ is parallel to $\textbf{n}$ and $\hat{\textbf{h}}$ (the angular momentum direction).

By crossing $\textbf{n}$ with the eccentricity vector we get:
$$
\begin{align}
\textbf{n} \times \textbf{e} &= \ell \textbf{s} \\
\textbf{s} \overset\Delta= (r_2 - r_3) \textbf{r}_1 + (r_3 &- r_1)\textbf{r}_2 + (r_1-r_2)\textbf{r}_3
\end{align}
$$
As $\textbf{n}$ and $\textbf{d}$ are both parallel to $\hat{\textbf{h}}$, so $\textbf{n} \times \textbf{e}$ must be in the $\hat{\textbf{q}}$, which means $\textbf{s} || \hat{\textbf{q}}$ .

This gives all the information needed to solve for all of the geomtric orbital parameters
Semi parameter
$$
\ell = \frac{||\textbf{n}||}{||\textbf{d}||}
$$
Eccentricity
$$
e = \frac{||\textbf{s}||}{||\textbf{d}||}
$$
where $\textbf{s}$, $\textbf{d}$ and $\textbf{n}$ are formed from the original measurements of the three $\textbf{r}_i$'s or via the calculations from Gauss's method

NOTE: This method only works if all three of the vectors are coplanar
Works well with large angular separations (opposite of Gauss)

This method if often applied with an initial application of Gauss. Keep going back and forth between Gauss and Gibb's as you converge on an orbital solution.