---
layout: post
title: Uncertain Agents
tags: [phd,pyagent,horace]
date: 2013-05-09
---
*Post is stil lbeing written*

It seems cruel to inflict uncertainty on a robot - the only thing the poor machines have going for them is unflappable certainty born from a lack of imagination. However, such is the life of Bayesian.

I am working on a simple model of a small robot (Horace), exploring a world. Horace can move one unit of space in each time period (it is free to choose the direction). Its control system is not perfect, however, and Horace's future position is related to its control input by a normal distribution. 

Assuming Horace's world can be expressed with a Gaussian Process, and making a further assumption for the sake of algebraic simplicity the Gaussian Process has a squared exponential covariance, we can come up with expressions for Horace's expected values of the GP's mean and variance functions by marginalising over space. 

\begin{equation}
\frac{1}{\sqrt{2} \begin{vmatrix}1 + \Sigma_s^{-1} \Sigma_c \end{vmatrix}^{\frac{d}{2}}} \left[e^{ - (\mathbf{Z}\_{d} - c)^T \Sigma^{-1} (\mathbf{Z}\_d - c)} \right]^T\mathbf{Kdd}^{-1} \mathbf{f}_{d}
\end{equation}


$$
\frac{\sqrt{\pi}}{\sqrt{2} \begin{vmatrix}\Sigma\_p^{-1}\left(1 + \Sigma_s^{-1} \Sigma_c\right)\end{vmatrix}^{\frac{d}{2}}} \left[e^{ - (\mathbf{Z}\_{d} - c)^T \Sigma^{-1}  (\mathbf{Z}\_d - c) - (\mathbf{Z}\_{d} - \mu)^T \Sigma\_v^{-1}  (\mathbf{Z}\_d - \mu) }\right]^T\mathbf{Kdd}^{-1} \mathbf{1}
$$

