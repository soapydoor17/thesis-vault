# Inputs
The two inputs required to produce Gauss-Newton matrix are an initial two line element set (TLE) and proceeding observations. For this example, the initial TLE is given as such:

```plain text
1 60956U 98067WW  24256.99968611  .00235683  00000+0  34489-2 0  9990
2 60956  51.6339 240.0861 0011344 359.3789  15.7195 15.54180488  2268
```

From the TLE, the following orbital parameters and datetime stamp can be found:

| Parameter                     | Value                   |
| ----------------------------- | ----------------------- |
| Semi-major axis ($a$)         | 6783182.731170822 m     |
| Eccentricity ($e$)            | 0.0011344               |
| Inclination ($i$)             | 51.6339 deg             |
| RAAN (${}\Omega{}$)           | 240.0861 deg            |
| Arg of perigee (${}\omega{}$) | 359.3789 deg            |
| Mean anomaly (${}M_{0}{}$)    | 15.7195 deg             |
| ISO Datetime stamp            | 2024-09-12T23:59:32.880 |

And the proceeding observations are imported from a `.xlsx` file, as seen in Appendix B. It provides the time of the observation (year, month, day, hour, minute, seconds as separate columns), the measured Doppler shift (Hz), and the central frequency (Hz). 

The timestamp at each observation can be used with the initial orbital parameters to calculate it's respective true anomaly ${}\nu{}$ as well as the state vector of the groundstation (${}\textbf X_{gs}=[\textbf r_{gs}, \ \textbf v_{gs}]^T{}$).  From there, the math previously shown can be used to get the Gauss-Newton matrix.
# Outputs
## Gauss-Newton Matrix from Single Doppler Measurement
The single doppler measurement used in this section is:

| Field                 | Value     |
| --------------------- | --------- |
| Year                  | 2024      |
| Month                 | 9         |
| Day                   | 13        |
| Hour                  | 7         |
| Minute                | 35        |
| Second                | 5.8524243 |
| Doppler Shift (Hz)    | -9.73E+03 |
| Centre Frequency (Hz) | 4.38E+08  |

Using this as the input gave the following values

*Ground Station State Vectors*

$$ \mathbf{r}_{gs} = \begin{bmatrix} -4022202.60251025 \\ -3632551.277273 \\ -3351356.54150797 \end{bmatrix} \text{m} \qquad \mathbf{v}_{gs} = \begin{bmatrix} 264.87946656 \\ -292.71712886 \\ -0.62329967 \end{bmatrix} \text{m/s} $$

*True Anomaly*
$$
\nu = 6.030164 \text{ rad}
$$

*Satellite State Vectors*

$$
 \mathbf{r} = \begin{bmatrix} -3020280.32564041 \\ -6044413.75274605 \\ 500780.32767482 \end{bmatrix} \text{m} \qquad \mathbf{v} = \begin{bmatrix} 4470.73818599 \\ -1738.65978667 \\ 5990.43721675 \end{bmatrix} \text{m/s} 
$$

*Jacobian Matrix*

$$
J = \begin{bmatrix} 1.56007496\times 10^1 & -4.20501806\times 10^{-3} \end{bmatrix} 
$$
*Gauss-Newton Matrix*
$$
 G = \begin{bmatrix} 6.31288011 \times 10^{2} & -2.00436196 \times 10^{-1} \\ -2.00436196 \times 10^{-1} & 6.36392078 \times 10^{-5} \end{bmatrix} 
$$
*Eigenvalues*

$$
 \lambda = \begin{bmatrix} 0.0 \\ 631.28807418 \end{bmatrix} 
$$
*Eigenvectors*
$$
 \textbf V = \begin{bmatrix} -3.17503553 \times 10^{-4} & -9.99999950 \times 10^{-1} \\ -9.99999950 \times 10^{-1} & 3.17503553 \times 10^{-4} \end{bmatrix} 
$$
## Gauss-Newton Matrix from Multiple Doppler Measurements
All of the Doppler measurements used in this section are shown in Appendix B.

If the values from the previous section are correct, then it can be assumed that the functions work as intended, and thus only the results from the Jacobian matrix onwards will be shown.

*Jacobian Matrix*

$$  
J = \begin{bmatrix}  
1.56007496 \times 10^{1} & -4.20501806 \times 10^{-3} \\  
1.55842813 \times 10^{1} & -4.19973428 \times 10^{-3} \\  
1.54993371 \times 10^{1} & -4.17257670 \times 10^{-3} \\  
1.53480081 \times 10^{1} & -4.12458914 \times 10^{-3} \\  
1.52995799 \times 10^{1} & -4.10933696 \times 10^{-3} \\  
1.51958858 \times 10^{1} & -4.07684685 \times 10^{-3} \\  
1.51439129 \times 10^{1} & -4.06064734 \times 10^{-3} \\  
1.51395790 \times 10^{1} & -4.05929904 \times 10^{-3} \\  
1.51143425 \times 10^{1} & -4.05145554 \times 10^{-3} \\  
1.51101183 \times 10^{1} & -4.05014396 \times 10^{-3}  
\end{bmatrix}  
$$
*Gauss-Newton Matrix*
$$
 G = \begin{bmatrix} 2.34234008 \times 10^{2} & -6.29233512 \times 10^{-2} \\ -6.29233512 \times 10^{-2} & 1.69034631 \times 10^{-5} \end{bmatrix} 
$$
*Eigenvalues*

$$
 \lambda = \begin{bmatrix} 7.59107722 \times 10^{-11} \\ 2.34234025 \times 10^{2} \end{bmatrix}
$$
*Eigenvectors*
$$
 \textbf V = \begin{bmatrix} -2.68634557 \times 10^{-4} & -9.99999964 \times 10^{-1} \\ -9.99999964 \times 10^{-1} & 2.68634557 \times 10^{-4} \end{bmatrix}  
$$