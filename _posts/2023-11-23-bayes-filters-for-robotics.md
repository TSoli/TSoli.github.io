---
title: Bayes Filters for Robotics
date: 2023-11-23
categories: [Robotics, State Estimation]
tags: [study, statistics, mathematics, probability, state estimation, linear systems, robotics]
math: true
toc: true
---

<!-- prettier-ignore -->
* TOC
{:toc}

## Incorporating Evidence Into Our Beliefs

At its core, Bayes' Theorem is a mathematical method for updating your belief about the state of
something given some evidence. For example, let's say I asked you to guess what fruit I am thinking
of. Your current belief incorporates possibilities for many different fruit such as apples, oranges,
bananas e.t.c. Now if I tell you the thing is brown and furry on the outside how might this inform
the probabilities you assign to different guesses? Well obviously its a lot less likely that I am
thinking about an apple right now since apples are not typically brown and furry on the outside.
Similarly, you would probably not guess that I am thinking of a cat or a dog just because I said the
thing is brown and furry on the outside - you also know it should be a fruit! It should be clear
that with each new piece of information, the number of "good" guesses \(let's define a "good" guess
as having some likelihood above an arbitrary threshold\) becomes smaller and the chance of the guess
with the highest likelihood being correct becomes greater. In this post I hope to show how we can
quantify this process.

I was thinking of a kiwi fruit by the way!

![A kiwi fruit](/images/kiwi_fruit.jpg) _A picture of a kiwi fruit also known as a Chinese
gooseberry_

## Bayes' Theorem

I have to start by linking [this amazing video](https://www.youtube.com/watch?v=HZGCoVF3YvM) by
[3Blue1Brown](https://www.youtube.com/@3blue1brown) from YouTube which does a fantastic job of
explaining the maths and intuition behind Bayes Theorem intuitively and visually. Feel free to check
it out below but nevertheless I will try to summarise it.

<center><iframe width="560" height="315" src="https://www.youtube.com/embed/HZGCoVF3YvM" title="Bayes theorem, the geometry of changing beliefs" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe></center>

So to begin, let's go over some terminology. In our previous example our hypothesis would be the
guess that we are going to make. We want to know what the probability is that it's correct. When I
had only told you that I am thinking of a fruit this probability would ha\ve been quite low - many
other fruit are more common and therefore its more likely that I would be thinking about them.
Therefore, let's say initially you assigned a probability that I am thinking of a kiwi fruit as only
0.005 or 0.5%. We will denote the probability of this hypothesis as $P(H) = 0.005$. This initial
probability of our guess is known as the _prior_ since it it represents our believe _prior_ to
considering some further evidence or information. To represent this probability _given_ some
additional information we would write $P(H|E)$ which represents our updated belief that we are
trying to solve. This is known as the _posterior_ since it represents our belief _after_ considering
the additional evidence or information.

![An image of a grid representing the sample space](/images/bayes_theorem.jpg) _An image from
3Blue1Brown's video on Bayes' theorem_

Next let's incorporate the new information or _evidence_ which was that the fruit is brown and furry
on the outside. This changes our belief in two ways. Firstly, we can restrict the total
possibilities to only include fruit that are brown and furry on the outside. Well kiwi fruit is
furry but so are coconuts and maybe some other fruit too. Let's say that overall only 0.75% of fruit
are considered brown and furry on the outside. Mathematically, we can say the probability of this
evidence is $P(E) = 0.0075$ out of all possible fruit.

Finally, we should consider the _likelihood_ that I would give this piece of evidence given that I
was, in fact, thinking of a kiwi fruit. I could have said it's green on the inside or a bit acidic
or any number of other things but if I was trying to help you guess correctly then I should give you
the most unique descriptor I can of the fruit \(we will assume this is the case here\). Given this,
let's say that 99.5% of the time that I was thinking of a kiwi fruit I would have given this
evidence. Mathematically we can say the probability that you received this evidence given that I was
thinking of a kiwi fruit is $P(E|H) = 0.995$.

Putting this all together we can say that the 0.5% of the time I was thinking of a kiwi fruit and
99.5% of the time I was thinking of the kiwi fruit I would have said it was brown and furry on the
outside. Therefore, the joint probability of these events \(the probab\ility that both occurred\) is
$likelihood \times prior = P(E|H) \cdot P(H) = 0.995 \cdot 0.005 = 0.004975$. We also know that only
0.75% of the time I would have used this description for the evidence and so we can divide by this
since the joint probability from before is clearly a subset of all the times this evidence was
given. Therefore, overall we have

$$
posterior = \frac{likelihood \times prior}{evidence}
$$

or

$$
P(H|E) = \frac{P(E|H) \cdot P(H)}{P(E)} = \frac{0.995 \cdot 0.005}{0.0075} = 0.66333
$$

In many cases instead of knowing the overall probability of the evidence we may instead know the
likelihood of the evidence given the hypothesis is true and given it is not true. In the context of
our example, we may instead know that 99.5% of the time I was thinking of a kiwi fruit I said it was
brown and furry but and 0.2537% of the time I was not thinking of a kiwi fruit I would have also
given this same information. Since this accounts for all possibilities when I gave this evidence,
they add to the probability that I gave that evidence. That is,

$$
P(E) = P(E|H) \cdot P(H) + P(E|\bar{H}) \cdot P(\bar{H}) = 0.995 \cdot 0.005 + 0.02537 \cdot 0.995 = 0.0075
$$

Where $\bar{H}$ represents the complement of $H$ \(all possibilities except the hypothesis\).
Therefore, another common form of Bayes' Theorem is

$$
P(H|E) = \frac{P(E|H) \cdot P(H)}{P(E|H) \cdot P(H) + P(E|\bar{H}) P(\bar{H})}
$$

## Applications in Robotics

While Bayesian filtering is applicable to many different fields in this article I will focus on how
it can be used in robotics for state estimation. State estimation is important in robotics because
processes can often be described using
[finite state machines](https://en.wikipedia.org/wiki/Finite-state_machine) or
[policies](https://en.wikipedia.org/wiki/Markov_decision_process#Policy_iteration) which map a
robot's state to an optimal action for it to take. Of course, for this to be effective, the robot's
state must be accurately estimated.

A typical example would be estimating a robot's position given information from encoders on the
wheels \(encoders would measure the amount of rotation of the wheels\) and some sensors that attempt
to match the environment to a map. We would like to combine these pieces of information so that we
can find the most likely position that we are in at a given time. This can be achieved in two steps
_prediction_ which gives us our prior and _correction_.

### Prediction

The prediction is the first step and essentially provides us with an estimate of the new position
based solely on the odometry information given the previous state. When applying Bayes theorem, this
becomes our _prior_.

We might begin with some initial position for the robot $\mathbf{x}$. Now we can model our robot
movement as an initial turn $\delta_{rot1}$ then a straight movement forward $\delta_{trans}$ and
then a final turn $\delta_{rot2}$ \(in reality the robot may have differential drive and not
actually move like this but this model allows any start and end poses and therefore can model any
potential paths\). The encoders can summarise the robot's movement over this time and present the
information given this model.

![Odometry motion model showing an initial rotation, then translation then final rotation](/images/odom_diagram.jpg)
_A diagram representing the odometry motion model_

Unfortunately, the values provided by the encoders cannot perfectly represent the robot's movement
due to things like slip on the tyres or misaligned wheels or noise in the sensors. Taken together,
we might assume that given the robot moved to some new pose $\mathbf{x'}$ the odometry information
reported may follow some probability distribution. If the actual position can be explained by the
actions $$(\hat{\delta}_{rot1},\, \hat{\delta}_{trans},\, \hat{\delta}_{rot2})$$ and we assume the
error in the reported odometry information is normally distributed with a mean of 0 and some
variance $\sigma^2$ then for each of the actions

$$
\delta = \hat{\delta} + \epsilon_{\sigma^2}
$$

where $\epsilon_{\sigma^2} \sim N(0, \sigma^2)$ \(note that $x \sim N(\mu, \sigma^2)$ means $x$ is a
random variable that is normally distributed with mean $\mu$ and variance $\sigma^2$\) represents
the error in the measurement. Furthermore, it is reasonable to assume that the variance would grow
with larger movements and depending on a specific robot. Taken from _Probabilistic Robotics_ by
Thrun, Burgard and Fox this can be modelled as

$$
\begin{align*}
  \sigma^2_{rot1} &= \alpha_1 \hat{\delta}^2_{rot1} + \alpha_2 \hat{\delta}^2_{trans} \\
  \sigma^2_{trans} &= \alpha_3 \hat{\delta}^2_{trans} + \alpha_4 (\hat{\delta}^2_{rot1} + \hat{\delta}^2_{rot2}) \\
  \sigma^2_{rot2} &= \alpha_1 \hat{\delta}^2_{rot2} + \alpha_2 \hat{\delta}^2_{trans}
\end{align*}
$$

where $\alpha_n$ are some robot specific parameters. So now if we have some hypothesised pose for
the robot, we can calculate actions $\delta$ that achieve this pose \(not discussed here for the
sake of brevity\), calculate the error $\delta - \hat{\delta}$ for each action and their variances
$\sigma^2$ and then plug it into the model for our probability distribution to get the likelihood
for each of these errors. For a normal distribution

$$
p(x) = \frac{1}{\sigma \sqrt{2\pi}} e^{-\frac{(x - \mu)^2}{2\sigma^2}}
$$

In this case, $x = \delta - \hat{\delta}$ is the error of the hypothesis from the odometry
information, $\sigma^2$ is the variance of the error and $\mu$ is the expected value of the error
which is 0 by assumtption. If we assume each of these errors occur independently, then their joint
likelihood is simply their product. That is

$$
p(\mathbf{x}) = p(\delta_{rot1}) \cdot p(\delta_{rot2}) \cdot p(\delta_{trans})
$$

where $p(\delta)$ is calculated using the error compared to the odometry as above.

### Correction

Next we want to incorporate the information from our sensors so that we can improve our certainty in
the position of the robot. The exact models used to do this based on Lidar scans or other things can
be complicated. For the sake of simplicity I will make a few general points about this. Firstly, the
likelihood of a given position given some sensor readings is itself an application of Bayes'
Theorem. That is because we only receive a measurement that _depends_ on the _real_ position of the
robot. In other words, for a sensor reading $\mathbf{z}$ we want to know $P(\mathbf{x}|\mathbf{z})$.
Practically, it is much more feasible to calculate $P(\mathbf{z}|\mathbf{x})$ - that is the
likelihood we received a particular measurement given a particular position. Using Bayes' Theorem,

$$
P(\mathbf{x}|\mathbf{z}) = \frac{P(\mathbf{z}|\mathbf{x}) \cdot P(\mathbf{x})}{P(\mathbf{z})}
$$

Assuming we have some model for $P(\mathbf{z}|\mathbf{x})$ we can then use our odometry prediction
as our _prior_ $P(\mathbf{x})$ as well. Then, since for all hypotheses for $\mathbf{x}$ we would
divide by $P(\mathbf{z})$. To calcualte this we can recall,

$$
P(\mathbf{z}) = P(\mathbf{z}|\mathbf{x}) \cdot P(\mathbf{x}) + P(\mathbf{z}|\bar{\mathbf{x}}) \cdot P(\bar{\mathbf{x}})
$$

And we can actually keep going with this since now we have a new prior distribution. We can make
another move and prediction and then update it based on the new measurements. This is known as
recursive state estimation. Of course, to do this you would now need to consider the contributions
from each possible starting state for a given hypothesis \(now our starting position is not fixed
and is itself a distribution\). In some cases this is easier than it sounds and in the future I hope
to write another post about how to achieve this using a Kalman filter.
