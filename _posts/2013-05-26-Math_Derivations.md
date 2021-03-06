---
date: 2013-05-26
tags: phd math 
layout: post
title: Maths of the Reliability model
---

Steve asked me to go into more detail about the maths I used to generate that _beautiful_ video in my last post.

The overall intention of this work is a small extension to [Gaussian Processes for Global Observation [gpgo] (GPGO). In GPGO it is possible to sample at any point in the objective function, and samples are guaranteed to be carried out at the intended location.

In a citizen science application we might have constraints on where the participant is willing to go, and on how likely they are to follow our instructions. We can model these with a probability distribution $p(\mathbf{z} \mid \mathbf{c})$ over locations $\mathbf{z}$ given an instruction or control input $\mathbf{c}$.

In a citizen science application, our $\mathbf{c}$ might be an instruction to the user to take a given route to work. This may not be the quickest route, so there is a chance they will ignore our request, be delayed by traffic or decide not to go to work that day.

In this work, we used a very simple model of a robot with an uncertain control system, constrained to be able to move approximately one unit of space in one unit of time. Given a control input $\mathbf{c}$, it attempted to reach that point by the next time step. We modelled the control system with a normal distribution, such that $p(\mathbf{z} \mid \mathbf{c}) = \mathcal{N}(\mathbf{z} \mid \mathbf{c}, \mathbf{\Sigma_c})$.

At each time-step, the robot decided where to make its next move by minimising a cost function over the control input $\mathbf{c}$, constrained so that $\| \mathbf{z_{now}} - \mathbf{c}\| = 0$. If we use a Gaussian Process (GP) to represent the robot's view of the world, one of the simplest functions to optimize would be the expected value of the mean of the GP for a control $\mathbf{c}$, given the distribution $p (\mathbf{z} \mid \mathbf{c}) $.

For a Gaussian Process $p(f \mid f\_D, z\_D, I)$ with a Normal kernel, the expected value of the mean function is

$$
\mathbb{E}_{c}(m(z)) = 
\int_{-\infty}^{\infty}
\int_f
f(x) p(f \mid f_D, z_D, I ) p(z \mid c ) \ df \ dz 
= \int_{-\infty}^{\infty}  m(z \mid f_D, z_D, I) p(z \mid c) \ dz 
$$

where $m(z \mid f\_D, z\_D, I) = K\_{CD}K\_{DD}^{-1}f\_D$, and $K\_{CD}$ is the vector of covariances of the control input $\mathbf{c}$ to sample locations $\mathbf{z}\_D$ and $K\_{DD}$ is matrix of covariances between sample locations. Given our Normal covariance kernel, we can write this as:

$$
\mathbb{E}_c(m(z))=
\int_{-\infty}^{\infty}
\mathcal{N}(z \mid c, \Sigma_c) 
\mathcal{N}(z_D \mid z, \Sigma_{gp}) 
K_{DD}^{-1}f_{DD} 
\ dz
$$

and using Normal identities rearrange to:

$$
\mathbb{E}_c(m(z))=
\int_{-\infty}^{\infty}
\mathcal{N}(z_D \mid c, \Sigma_c + \Sigma_{gp}) 
\mathcal{N}(z \mid \mu, \Sigma_c - \Sigma_c(\Sigma_c + \Sigma_{gp}) ^ {-1} \Sigma_c) 
K_{DD}^{-1}f_{DD} 
\ dz
$$

Finally we reorder the integral and marginalise over $\mathbf{z}$ to get:

$$
\mathbb{E}_c(m(z))=
\mathcal{N}(z_D \mid c, \Sigma_c + \Sigma_{gp}) 
K_{DD}^{-1}f_{DD}
$$

The videos presented in the previous blog post were generated by locally minimising this function. The interesting part is the change in behaviour of the two agents which start in the same location and differ only in $\Sigma_c$. 

[gpgo]: http://www.robots.ox.ac.uk/~mosb/papers/OsborneGarnettRobertsGPGO.pdf "M. A. Osborne, R. Garnett and S. J. Roberts (2009). _Gaussian processes for global optimization_"