---
title: Hunting Down Ignorance
date: 01-06-2013
tags: phd math horace
layout: post
private: true
---

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

Writing that equation in code was remarkably difficult - two nested loops would have been the easiest way, but NumPy is insufferably slow when used in this way. It's just a case of having to think in new ways.

The behaviour from this equation should be to head towards places with high levels of uncertainty. This is myopic local optimisation.
