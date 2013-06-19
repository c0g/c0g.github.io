---
title: GPGO with expected loss
date: 01-06-2013
tags: phd math horace
layout: post
private: true
---

The global part of GPGO comes from the \eqref{exp_loss}, the epectation of the loss function of \eqref{loss}. $\eta$ is the current minimum of the function of interest. 

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

We now need to marginalise this over our agent's distribution on future locations.
$$
\label{exp_loss_marg}
\mathbb{E}(\Lambda(x)) = \int_{\infty}^{\infty} \eta + (m(x) - \eta) \Phi(\eta \mid m(x), C(x)) - C(x) \mathcal{N} (\eta \mid m(x), C(x)) p(x \mid c) \, dx 
$$

which leaves us trying to marginalise with respect to a very nasty distribution. To proceed, we take the Laplace approximation where we replace $\Lambda(x)$ with a normal distribution, giving $\tilde{\Lambda}(x) = N(x \mid \mu, \Sigma)$.



[GPGO]: http://www.robots.ox.ac.uk/~mosb/papers/OsborneGarnettRobertsGPGO.pdf "M. A. Osborne, R. Garnett and S. J. Roberts (2009). _Gaussian processes for global optimization_"