---
layout: post
title: Uncertain Agents
tags: phd
date: 2013-05-13
---
It seems cruel to inflict uncertainty on a robot - the only thing the poor machines have going for them is unflappable certainty born from a lack of imagination. However, such is the life of Bayesian.

I am working on a simple model of an agent (Horace), exploring a world. Horace can move one unit of space in each time period (it is free to choose the direction). Its control system is not perfect, however, and Horace's future position is related to its control input by a normal distribution. 

The purpose of this model is to include a measure of trust in an Agent's assesment of what it believes it can achieve. At each time step, the agent minimizes it's loss function for the next move. In a multi agent system, the agent with a more certain control system would most likely get precedence over the others.
