---
Assigned: 2026-08-11
Due: 2026-08-25
Status: Active
Type: dummy-problems
tags:
  - task
---
# Task
## Problem 1
For a pendulum acting under gravity and starting at a small deviation angle from down $\theta_0$, the equation of the period is:

![](data:image/png;base64,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)

_without rearranging_, use the [[Gradient Descent]] algorithm to solve for the pendulum length $L$ given the period $T$ is measured to be 5 seconds (or any arbitrary value).

Check these results against the analytical solution.

## Problem 2
For a pendulum acting under gravity and starting at an arbitrarily large deviation angle from down $\theta_0$, the equation of the period is:

![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAALIAAABHCAYAAACj1R3CAAAJ20lEQVR4Xu2cx8sVPRSH5/0LxLIWsSxcKaIIooIu7IggdhEXYl8pFiyI2BU3IjYURBQ7gqCoCwUbiAVFd5aFuLT+BX48g2e+mJtkMnPnlm++88CLzmTuzUzyy8nJybnT8/v379+JovzH6VEhK3VAhazUAhWyUgtUyEotUCErtUCFrNQCFbJSC1TISi1QISu1QIWs1AIVslILVMhKLVAhl+Dx48fJuHHjsuM8NC+r9aiQS3D79u3k3bt3ycaNG7NzSmdRIZfg4MGD6b8q5O5BhVwChNy/f/9k/vz52Tmls6iQS4CQx4wZk4wdOzY7p3QWFXIJVq9enSxcuFCF3EWokEswZcqUZNu2bSrkLkKFXAKEfOHChaRv377ZOaWzqJBLgJDv3LmTHQN+86ZNm9L/Hzhw4H8X0Xjz5k36/Hfv3k0GDRqUXL9+PRk2bNif1mk9KuQS9OnTJ/n+/Xt2LIwaNSq10rbI686nT5+SkSNHJvv27UtWrFiRDupXr14lly5datujq5BL0NPT49ytGzx4cLJ79+62h+WwhleuXEn27NmT3Usrsetjhpo4cWI2CyHgs2fPBge0iNwVwgyV+VAhl8DlWmCVmFI/fvyYDBw4MDvfar59+5bez+XLl9tSr12fWOP3799na4atW7cmL1++bGgjG6I/48ePdwo2VOZChVwCl5CxIkQyPnz4kJ1rB3T0rFmzoju8Wez6Tp48maxcufJP6b+sWrUqOXbsWHbsgkExevTo5N69ew2DMFTmQoVcEBKGcB9sIWNBIK/zqoR7Wbp0adsGj6s+260AXK8TJ06k/nIeGICHDx862y1UZqNCLgidefTo0cyPE1joLVu2LKrzqgIRIax2WWNXfTz3zp07k2nTpqXHtMuCBQsyFwt/evbs2enxli1bGvx4LO+QIUOSFy9eNFjeUJmNCrkgCPnp06d/WSAavF+/fsnr16+zkBMd+vnz55aF4RDI8OHDk69fv7Ylnu2rD+v76NGjbHMIYfMnVpQFMAu/oUOHpq7CkSNHMtELodksVGaiQi6IK4WTc4sXL85CcrIAunnzZst2/1hQYeXsmaFV+OozXQtER7ks/ET8EuHhO3r16tUwuO32MwmVmaiQC2KncMoqnunPxhWiqwos3YYNG9rmyvjqM12HyZMnp5tBMivJDxCkHey2M8Gy37p1q8FaQ6hMqEzIdOiZM2eSX79+NfhBsTCace537drVlumyDN2QwimhPnNKbyVl64u1yOBaNAqhMsErZHPL1YUZLxWrtH79+oaFB9+zf//+5MePH05n30ZGeGzYpVnEasR2Es9TZQqn1A9YNKIhrnMmtqWLocxnhGY+yy7o+fPnU/+YP/xlV9vhljBg7GeFUJngFTLCREg42WaAm4eaOXPmXz6Lb6RxHkESPKecFeiDBw9y9+CxzHk7Q1VgugWxQqZRq07hpJ1oo2fPnmUzEQIgFrtu3bqG2UmMjKfrnDQjxjL1CfQlbZZnyMTguXzhUJngFTIrz+fPn2f/nzNnTiZUOl9EJit200KDCN7c8ZEVbIwIilxbFmaPJUuWJNOnT48WMs9edQon98HUTSfTnmvWrEnvy+cT0rH3799v+UBvZ32yseKSY6hM8ApZEKH6OpoRd/jw4Uz0AqNwwIABmfjzvseGz/fu3ds7gpuF1fC5c+fS+7dDSCEQctUpnFhfIhxfvnyJWiNwD+CasXgeBhqGBbCCc+fOTSZMmJBaRbob902OCRlK1ppvNy5UX1XIjGGH9yBUJuQKmQ7HYvkuY1p0hWXoHBrKxrbcPvKsDo1L44fwiZNBhb/GVI54WJD4rrUxZ6MqkAWRcPHixYZ1hk1IWOYK33TRbNdCjhE67gttQT+bsXAhVF9VyP24+iFUJuQK2fSPXfCQ9orSbjSI8XNM8oTcDAhl7dq1aaOYjeTaeraxUzgRTh6hJmbaPHToULrtGyuY0HU+39ruE/sYfDOTr76YZ/dht0lIrKEyIVfItn9s4xIyVnzHjh1/uRv4vJMmTcqmLgbI3r17vUnYrRKybQFtcpoj7by8a4rAoEJ8tIt0WN6s5RMW8HymqyBuii1c+xiKCrlKQmINlQlBIcf4tS7XwhahuCfSQeaUh0U6ffp0g49tf4cNjVvWtTCJaSQT6vXdUxlExOJOMOCXL1/uNRwQIyzCVfPmzcu2i23h2sdQWyHLajHUyYjSXuzRSBKxwAdlYbF58+asc+i0ESNGpMcyWOzx1OrFntBJIcsAN3etGMCnTp0KZrT5hMyz4B7JYlQGB30kdcmCyT6WfnDtoPnqqxLpB9eCLlQmeC2yfBgQlM+3lQawp0NxHfisKWKgYUx3xDVdtyP8JvVw77g4IfGATNtVdaj4mGbdMtNgTe0FtOCbrbg/MvBku5x8D2Y73DZZfMv32seInli2q6999VVJKMQWKhO8Qi4CooVY65lnkU3Xo5tgcLtSONsNwiq7QVGGIvXRNszQDCYGBX56jC6owxcMCJUJlQgZMWJJXFvULhhhN27cSKfAa9eupf8X0WJV2rlFXQSEbKdwdgKZLSvouihi6+M68pUlvQBRk5scE1IMbUOHyoRKhAyIOTZpiGsXLVrU8NNxHjxmQ6BTdIuQyybxlKWZ+nCfYl6PYLubJqEyoTIh/x9gigNfg8oUKHkFWCY7GlMV+PautMpWUaY+cRtjLDKCdy00IVQmqJALgFB9b+FknXD8+PEsmZ5rfv78GZwOm8EV9mwlZeqjva5evZo7mImg+JLnQ2UmKuQC0DGuFE7X1GvnmlSNbOyEQlJVUrQ+WfQxkPOup63AlecRKjNRIRcAqzR16tQGIZvbzAJTsev3aVWC72j/GLSVxNYnYcAYEeN++H5gGiqzUSEXgI50pXBiqc04q8Q9Y61XWSRKkBf/roqY+iTqZOZWh5AFvsvihspsVMgFQMiuFE6EzG4cncffkydP0s2FUIdXBdaxky9osWFLXDZhAGH7XueFxfW9hCVU5kKFXACE7Fq84SOTEMWCBOvx9u3bwgujstDh3FenXpllwvMSN7bxhd/wf32vxQqVuVAhFwC/17ayNPiMGTP+8oW5rp0vMwxZvVZQRX0yyF1CDZX5UCE7wBdEiFhXc1pz5YRwTuKkWMft27en06EteKW1qJAtWKjxQ1kWdSTdmO6By7WQiAWuBJCEQ7JUjF+nVIcK2YP4e2Zs2CVkpTtQIQfA1+UP8eIXVpnCqVSLCjmAJJ9jlaEbUjgVNyrkHHAnAJ+5G1I4FTcq5BwkF5cE8VbmTijNoUKOAKtM7rQvsK90HhVyBGKVY/Jqlc6gQo6EHbyqX16oVIcKWakFKmSlFqiQlVqgQlZqgQpZqQUqZKUWqJCVWqBCVmrBP1QHs6RLqnJNAAAAEGRlQkcyODk0RURGRTY2NTMwRjJEpUt5WwAAAABJRU5ErkJggg==)

where $K$ is the complete elliptic integral of the first kind ([Elliptic integral - Wikipedia](https://en.wikipedia.org/wiki/Elliptic_integral#Complete_elliptic_integral_of_the_first_kind "https://en.wikipedia.org/wiki/Elliptic_integral#Complete_elliptic_integral_of_the_first_kind")). Use the gradient descent algorithm to solve for $\theta_0$ when the length $L$ = 2.6 m  and the period $T$ is measured to be 5 seconds (or any arbitrary values).

## General Procedure
Define some error function that is positive everywhere except at your target state. The common choice is $E(y) = 0.5(\hat{y} - y)^2$, where $\hat{y}$ is your predicted observable calculated from your formula (in the case of problem 1, a predicted $T$ given your prediction for $L$), and $y$ is the real observation (measured $T$).

Through the chain rule, find how your error changes at every update step k (i.e. dE/dk) as a function of how your prediction changes at every update step (i.e. d(theta)/dk).

Since your error is positive everywhere and 0 when your guess matches your observation, if you can choose d(theta)/dk to force dE/dk to be negative everywhere except when your guess matches your observation, resulting in zero error (a correct prediction).

The only thing to be careful of is the elliptic integral function. You'll need to take its derivative, which you can find in the wikipedia page (given as d/dk(K(k))).

# Approach

### Task 1
Started off with a linear regression approach. Inputs an array of target T values. Makes a prediction for the value of L through w and b ($L = wX +b$) and the calcuates the predicted value of T given L_pred. Gradient descent then updates values of w and b, and the process repeats for n epochs.

Created a second, similar algorithm, except the predicting equation was: $L=w_1 * X^2 + w_2 * X +b$ which allowed for the modelling of the non-linear nature of the relationship between L and T

$$
E(L) = \frac12 (T_{pred}-T_{true})^2
$$
$$
\frac{dE}{dL} = (T_{pred} - T_{true}) * \frac{dT}{dL} = (T_{pred} - T_{true}) * \frac{\pi}{\sqrt{g*L_{pred}}}
$$

### Task 2
This doesn't model the relationship between $\theta_0$ and $T$, but rather given a single value of T, uses gradient descent to directly the value of $\theta_0$. 

NOTE: K(k) = `ellipk(k**2)` from scipy (complete elliptic integral of the first kind) and E(k) = `ellipe(k**2)` (complete integral of the second kind.)

$$
E(\theta_0) = \frac12 (T_{pred}-T_{true})^2
$$
$$
\frac{dE}{d\theta_0} = (T_{pred} - T_{true}) * \frac{dT}{d\theta_0}
$$
$$
\frac{dT}{d\theta_0} = 4\sqrt{\frac Lg} * \frac{dK}{dk} * \frac{dk}{d\theta_0}
$$
$$
k = \sin(\frac{\theta_0} 2), \quad \frac{dk}{d\theta} = \frac12\cos(\frac{\theta_0}2)
$$
$$
\frac{dK}{dk} = \frac{E(k)}{k(1-k^2)} - \frac{K(k)}{k}
$$

