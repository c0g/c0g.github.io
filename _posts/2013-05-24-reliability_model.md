---
layout: post
title: Modelling agent reliability
tags: phd horace
date: 2013-05-24
---
I'll start by explaining why I am doing this particular piece of work - Mike has rightly warned me about having fun solving made up problems. 

My work at the moment is on sensing the real world using Citizen Science. The theory goes, people will volunteer to give up some of their time to various projects. They will be asked to go a certain place at a certain time, or take a slightly different route to work in order to gather data for some noble goal. Two such goals are sensing biodiversity using sound, or air quality using small bluetooth sensors.

We can't rely on people to do what we tell them all the time. We need to be able to model how likely someone is to follow our instructions, and customise the task we give them accordingly. It's this notion of _trust_ that I'm working on with this model. A less trusted person might only be given tasks that require a small amount of effort, or a small detour from their normal route. Alternatively, they may be given less important tasks, with the more important/interesting tasks reserved for the more dedicated.

----------

The above is the justifcation. Now for the fun...

The phenomena we'll be aiming to sense will be modelled as scalar fields over space and time. Think of the elevation contours on ordnance survey maps - the scalar field here is the height of the land above sea level. There's an obvious dependance on space here, but also [probably] on time. A fun excersise for the reader might be to steal a pile of historical ordnance survery maps and make a giant flip book.

Scalar fields such as these can be represented by Gaussian Processes (GPs). These provide estimated values of the field between obserations, along with assesments of how reliable the estimates are.

For a given location, we can easily ask the GP what we expect the value of the field to be - this is simply the mean function of the GP, and is easy to evaluate. However, if our human sensor hub can't quite be trusted to be in the correct location, we need to marginalise over all possible values of the mean function, to give us the expected value at the point $\mathbf{c}$:

$$
\mathbb{E}(m(c)) = 
\int_{-\infty}^{\infty}
\int_f
f(x) p(f \mid f_D, z_D, I ) p(z \mid c ) \ df \ dz 
= \int_{-\infty}^{\infty}  m(z \mid f_D, z_D, I) p(z \mid c) \ dz 
$$

If we use a Normal distribution for both the covariance matrix of the GP and the distribution over possible locations, this works out to be a weighted sum of normal distributions:

$$
\mathbb{E}(m(z))=
\mathcal{N}(z_D \mid c, \Sigma_c + \Sigma_{gp}) 
K_{DD}^{-1}f_{DD} 
\label{eqhasSigmaC}
$$

A similar calculation can be performed to work out the variance at a given point.

These equations provide a way to take into account how likely an agent is to sample where we have told it to. The modified mean and variance functions they provide can be used to calculate the expected loss (or gain) for a move.

As a quick test, I simulated a small mobile sensor in a blobby, time changing world. The sensor is limited to moving one unit of space per unit of time. It doesn't like red, and is named Horace:

<video width="1000" src="http://www.robots.ox.ac.uk/~trn/horace.mp4" controls>horises</video>

The aim was to minimize the expected mean function (_not_ the expected loss). In the video, red represents something bad and blue something good. The left panel is the world, the middle panel is an agent that is very confident in it's ability to go where it needs to, the right is much less confident. This video mainly looks cool... but it is gratifying to see that the behaviour changes when the only thing that changes is $\Sigma_c$ in \eqref{eqhasSigmaC}.

<video width="1000" src="http://www.robots.ox.ac.uk/~trn/two_agent.mp4" controls>two horises</video>

### Next Steps:

* Modify to use expected loss/gain.
* Look into communication between agents to decide precedence for investigating a certain area.

