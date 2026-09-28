The Doppler model, ${}f_{D}(M_{0}, \ a){}$, is a complicated, nonlinear relationship involving Kepler's equations, rotations and the ground station geometry. It has been linearised by replacing the model with the tangent (a first-order Taylor expansion) near a chosen point ($\boldsymbol \theta_{\text{true}} = (M_{0 \; \text{true}}, \ a_{\text{true}})$):
$$
f_{D}(\boldsymbol \theta_{\text{true}} + \boldsymbol\delta) \approx f_{D}(\boldsymbol \theta_{\text{true}}) + J \boldsymbol\delta
$$

As the linearisation is done at the point of truth, the squared error is zero, meaning $E \approx \tfrac{1}{2} \boldsymbol\delta^{T}G\,\boldsymbol\delta$, so ${}G{}$ describes the shape of a quadratic bowl around the true parameters and is an approximation of the Fisher Information Matrix, which shows how observable each combination of parameters are from the data. The greater the change in the loss is, the more information that direction carries. The eigenvectors are the directions in the parameter space, where each column is a different combination of the parameters (${}M_{0}{}$ and ${}a{}$). The eigenvalues are the steepness along each axis, where a larger eigenvalue of a direction shows that changing it greatly affects the predicted Doppler shift, and a near-zero eigenvalue means the predicted Doppler shift barely changes along that direction, so the data cannot distinguish parameter sets that differ only along it.

For the single measurement, the eigenvalues show us that there is one direction in the parameter space that greatly informs the Doppler shift prediction (${}\lambda_{2}\approx 1522.16{}$), and one direction that does not (${}\lambda_{1}=0.0{}$). This makes sense as one measurement cannot independently determine two parameters. The eigenvector associated with ${}\lambda_{1}{}$ and ${}\lambda_{2}{}$ respectively are:
$$
\textbf{V}_{1} \approx \begin{bmatrix}
0 \\ -1
\end{bmatrix} \qquad
\textbf{V}_{2} \approx \begin{bmatrix}
-1 \\ 0
\end{bmatrix}
$$
Thus, the eigenvector of ${}\lambda_{1}{}$ points almost entirely in the direction of ${}a{}$, and ${}\lambda_{2}{}$'s eigenvector mostly points in the direction of ${}M_{0}{}$. For a single Doppler shift measurement, the observation provides almost no independent information on ${}a{}$, but is strongly sensitive to the value of ${}M_{0}{}$.

Using multiple observations, we see ${}\lambda_{1}{}$ shifted a little bit, but still near-zero, and ${}\lambda_{2}\approx 749.31{}$. Their respective eigenvectors are:
$$
\textbf{V}_{1} \approx \begin{bmatrix}
0 \\ -1
\end{bmatrix} \qquad
\textbf{V}_{2} \approx \begin{bmatrix}
-1 \\ 0
\end{bmatrix}
$$
This, again, shows that $a$ doesn't greatly affect the predicted Doppler shift, while the value of $M_{0}$ can strongly be observed. This is supported by the fact almost all of the rows in the multi-observation ${}J{}$ are approximately being proportional to each other. Despite there being more data, the ten observations also struggle to independently constrain $a$.

The multi-observation's struggle to observe the value of $a$ is likely because the ten measurements span over 52s, under 1\% of the roughly ${}\frac{2\pi}{n}\approx 5560{}$s orbital period. The satellite wouldn't have moved very far within this time frame and thus all of the information past the first row is mostly redundant and won't help form any new meaningful relationships.