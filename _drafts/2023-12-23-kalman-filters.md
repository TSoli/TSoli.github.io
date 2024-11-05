---
title: Kalman Filters
date: 2024-01-07
categories: [Mechanics, Control]
tags:
  [
    study,
    mechanics,
    dynamics,
    mathematics,
    linear systems,
    control,
    state space,
    state feedback,
    state estimators,
    observability,
    kalman filter,
    optimal state estimation,
    gaussian distributions,
    robotics,
  ]
math: true
toc: true
---

<!-- prettier-ignore -->
* TOC
{:toc}

## Making Optimal Estimations with Noisy Measurements

In my previous post on [Observability and
Detectability]({% post_url 2023-12-06-observability-and-detectability %}) I explained how we can
estimate the state of a system by using feedback from the outputs of the system with predictions
from our model of the system. However, at the end we found that there was a tradeoff between the
speed that the error in the model would converge \(or how quickly it would respond to disturbances\)
and the amount of noise in the state estimates. So, how can we choose an optimal gain for our
observer so that it's predictions are most accurate even with the noisy measurements? Well a Kalman
filter _is_ this observer with optimal gain if the noise fits a
[Gaussian \(or Normal\) Distribution](https://en.wikipedia.org/wiki/Normal_distribution).

In fact, Kalman filters can also be viewed as a special case of a
[Bayes filter](https://en.wikipedia.org/wiki/Recursive_Bayesian_estimation) which I discussed in
[another post]({% post_url 2023-11-23-bayes-filters-for-robotics %}) where the underlying
distribution is a Gaussian. At the end of that post we ran into a tricky situation where to keep
making recursive state estimations we would need to sum over all of the possible previous states to
get the likelihood of the current hypothesis. Once again, a Kalman filter allows us to do this
efficiently.

So in a lot of ways, this post is a culmination of many of the topics I have covered recently and
combines them to make quite a powerful tool that is useful across many fields and applications.

## Some Properties of Gaussian Distributions

As mentioned above, Kalman filters use Gaussian distributions to model the noise in systems and so
to understand how they work, it is important to recall some useful properties of Gaussian
distributions.

## Useful Resource

For this post I relied a lot on _Optimal State Estimation_ by Dan Simons. If you want a more
detailed explanation of many of the concepts from this and my recent posts on control theory, this
is a great resource.
