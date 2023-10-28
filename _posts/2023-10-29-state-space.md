---
title: State Space System Modelling
date: 2024-10-29
categories: [Mechanics, Dynamics]
tags:
  [
    study,
    mechanics,
    dynamics,
    Euler-Lagrange,
    mathematics,
    control,
    state space,
    Ordinary Differential Equations,
    ODEs,
  ]
math: true
---

<!-- prettier-ignore -->
* TOC
{:toc}

## Dynamical Systems

Many systems can be modelled using sets of
[Ordinary Differential Equations (ODEs)](https://en.wikipedia.org/wiki/Ordinary_differential_equation).
For example, in [my previous article]({% post_url 2023-07-28-Euler-Lagrange-basics %}) on the
Euler-Lagrange equation, we found that the equations of motion for particles in a system follow the
path of least action which turns out to be an ODE. Systems that can be modelled in this way are
known as [dynamical systems](https://en.wikipedia.org/wiki/Dynamical_system) and are ubiquitous
across many fields.

## Some Intuition About State Space

Consider the ODE

$$
x = \frac{dx}{dt}
$$

If I know the position of $x$ at some time, then I can calculate the derivative and using the
derivative I can update the position over some small amount of time. This allows me to get the
derivative at the new position and so on until I have sketched out a path.

<!-- prettier-ignore -->
![](/images/simple_ode_sketch.jpg) _Sketching out a simple ODE_

A general ODE is of the form,

$$
\frac{d^nx}{dt^n} = a_0 + a_1 x + ... + a_{n - 1} \frac{d^{n-1}x}{dt^{n-1}}
$$

Therefore, if we know the value of all the derivatives up to the $n$th at a specific time we can
calculate the $n$th derivative. Using this we can update the $n - 1$th and since we also know the
$n - 1$th derivative \(since we used to to get the $n$th\) we can get the $n - 2$th derivative
e.t.c. which means we can update all of the derivatives. Therefore, these derivatives down from the
$n - 1$th are known as the _state_ of the system since they contain all of the instantaneous
information required to update it over time. If we know what the derivative of each of the states
are in terms of the current state, then we know how the system will evolve over time.

For a set of $m$ generalised coordinates we can define the system states using each of their $n - 1$
derivatives in a state vector, $\mathbf{x}$.

$$
\mathbf{x} = \begin{pmatrix} q_1 \\ \frac{dq_1}{dt} \\ \vdots \\ \frac{d^{n - 1}q_m}{dt^{n-1}} \end{pmatrix}
$$

And then if we the states derivative in terms of the state we can write,

$$
\mathbf{\dot{x}} = \begin{pmatrix} \frac{dq_1}{dt} \\ \frac{d^2q_1}{dt^2} \\ \vdots \\ \frac{d^n q_m}{dt^n} \end{pmatrix} = \mathbf{Ax}
$$

For some matrix $\mathbf{A} \in \mathbb{R}^{k \times k}$ where $k$ is the total number of states.

## Adding Inputs

Often, we would like to manipulate or control our system using some external inputs that affect some
of the states. For example, we might be able to push the accelerator in a car to apply some force
causing it to accelerate. The chosen force will clearly affect how the car moves and will
instantaneously affect the car's velocity. For an effective autonomous car we might like to control
the car's position via this force.

$$


$$

$$
$$
