---
title: Expected Loss
date: 01-06-2013
tags: phd math horace
layout: post
private: true
---

The global part of GPGO comes from the \eqref{exp_loss}, the expectation of the loss function of \eqref{loss}. $\eta$ is the current minimum of the function of interest. 

$$
\label{loss}
\lambda(y) = \twopartdef {y} {y < \eta} {\eta} {y \geq \eta}
$$

$$
\label{exp_loss}
\Lambda(x) = \int \lambda(y) p(y \mid x, I) \ dy
$$

\eqref{exp_loss} encourages exploration in regions of high uncertainty, and exploitation of the current minimum when appropriate. (This is all copied from [GPGO] by Mike Osborne). After some maths, denoting the Gaussian CDF as $\Phi$ and the mean and covariance at $x$ as $m$ and $C$,

$$
\label{exp_loss2}
\Lambda(x) = \eta + (m - \eta) \Phi(\eta \mid m, C) - C \mathcal{N} (\eta \mid m, C)
$$

This gives us the expected return of evaluating the function at the point x. This is minimised in regions of low mean, and high uncertainty ($m$ lower than $\eta$, $C$ big respectively).

We have already [derived the mean function](/2013/05/26/Math_Derivations.html) for an untrustworthy agent, however we also need a covariance. The expression for the posterior covariance for a single point $z$ is:

$$
\sigma(z)^2 = K_{zz} - K_{zd}K_{dd}^{-1}K_{dz}
$$

where $K\_{zz'} = K(z, z')$. Writing this in sum form for N observations, with the Normal kernel explicitly shown and the matrix $\mathbf{W} = K\_{dd}^{-1}$:

$$
\sigma(z)^2 = \mathcal{N}(z \mid z, \Sigma_{gp}) - 
\sum\limits_{i=1}^{N} \mathcal{N}(z \mid d_{i}, \Sigma_{gp}) 
\sum\limits_{j=1}^{N} \mathcal{N}(z \mid d_{j}, \Sigma_{gp}) \mathbf{W}_{ij}
$$

for a control input $c$ and $p(c \mid z) = \mathcal{N}(c \mid z, \Sigma_{c})$,

$$
\sigma(c)^2 = 
\int\limits_{-\infty}^{\infty}
\left(
\mathcal{N}(z \mid z, \Sigma_{gp}) - 
\sum\limits_{i=1}^{N} \sum\limits_{j=1}^{N}
\mathcal{N}(z \mid d_{i}, \Sigma_{gp}) 
\mathcal{N}(z \mid d_{j}, \Sigma_{gp}) 
\mathbf{W}_{ij}
\right)
\mathcal{N}(c \mid z, \Sigma_c)
\ dz
$$

using Gaussian identities and [a bit of python](/normality.html) we get (for the integral)


$$
\int\limits_{-\infty}^{\infty}
\left(
\mathcal{N}(z \mid z, \Sigma_{gp})
\mathcal{N}(z \mid c, \Sigma_c) -
\sum\limits_{i=1}^{N} \sum\limits_{j=1}^{N}
\mathcal{N} ( y \mid d_i,\Sigma_c+\Sigma_{gp}) 
\mathcal{N} ( d_j \mid (d_i+\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}(c-d_i)),\Sigma_{gp}+(\Sigma_{gp}-\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}\Sigma_{gp})))
\mathcal{N} ( z \mid (d_i+\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}(c-d_i))+(\Sigma_{gp}-\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}\Sigma_{gp}))(\Sigma_{gp}+(\Sigma_{gp}-\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}\Sigma_{gp})))^{-1}(d_j-(d_i+\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}(c-d_i))),(\Sigma_{gp}-\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}\Sigma_{gp}))-(\Sigma_{gp}-\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}\Sigma_{gp}))(\Sigma_{gp}+(\Sigma_{gp}-\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}\Sigma_{gp})))^{-1}(\Sigma_{gp}-\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}\Sigma_{gp})))
\right)
\mathbf{W}_{ij}
\ dz
$$

wow that's horrible... luckily the long part marginalises out leaving:

$$
\sigma(c)^2 = 
\mathcal{N}(z \mid z, \Sigma_{gp})
-
\sum\limits_{i=1}^{N} \sum\limits_{j=1}^{N}
\mathcal{N} ( c \mid d_i,\Sigma_c+\Sigma_{gp}) 
\mathcal{N} ( d_j \mid (d_i+\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}(c-d_i)),\Sigma_{gp}+(\Sigma_{gp}-\Sigma_{gp}(\Sigma_c+\Sigma_{gp})^{-1}\Sigma_{gp})))
\mathbf{W}_{i,j}
$$

Writing that equation in code was remarkably difficult - two nested loops would have been the easiest way, but NumPy is insufferably slow when used in this way. However, it is written. Writ.

Conjugation aside, we can now test more interesting behaviour with the little Horises. A pure information scavenging mode (maximise variance), a frightened scavenger (expected loss) or what I'm going to call "the ostrich", which aims to avoid regions of high variance (hint: it will not move). Should be ready for the Friday update.

[GPGO]: http://www.robots.ox.ac.uk/~mosb/papers/OsborneGarnettRobertsGPGO.pdf "M. A. Osborne, R. Garnett and S. J. Roberts (2009). _Gaussian processes for global optimization_"
[julia]: http://www.julialand.org
