---
title: State Space System Modelling
date: 2023-10-29
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
toc: true
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
![ODE sketch](/images/simple_ode_sketch.jpg) _Sketching out a simple ODE_

Unlike frequency/Laplace methods, state-space equations can be developed for non-linear systems and
used to form the basis of non-linear control. Despite tihs, for this post I will focus on linear
state-space equations since they are easier to begin with.

A general linear ODE is of the form,

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
The same can be said for discrete systems if we know the state at the next time step as a linear
function of the current time step. These are known as difference equations as opposed to
differential equations.

## Inputs

Often, we would like to manipulate or control our system using some external inputs that affect some
of the states. For example, we might be able to push the accelerator in a car to apply some force
causing it to accelerate. The chosen force will clearly affect how the car moves and will
instantaneously affect the car's velocity. For an effective autonomous car we might like to control
the car's position via this force. The affect of the input, $\mathbf{u}$ on the state derivative can
be added in the matrix state space equation.

$$
\mathbf{\dot{x}} = \mathbf{Ax + Bu}
$$

For $k$ states and $i$ inputs $\mathbf{B} \in \mathbb{R}^{k \times i}$.

## Outputs

For the system we may have some desired outputs $\mathbf{Y}$ that are a function of the state and
perhaps the current inputs. Therefore, we have

$$
\mathbf{Y} = \mathbf{Cx + Du}
$$

For $p$ outputs and $k$ states we have $\mathbf{C} \in \mathbb{R}^{p \times k}$ and for $i$ inputs
$\mathbf{D} \in \mathbb{R}^{p \times i}$.

## State and Memory

Our overall system can now be described using two sets of matrix equations.

$$
\mathbf{\dot{x}} = \mathbf{Ax + Bu} \\
\mathbf{Y} = \mathbf{Cx + Du}
$$

We can think of the system as having some components with memory that inform the current state and
some components that are memoryless and cause an instantaneous output. The second equation
demonstrates this well where $\mathbf{x}$ acts as the memory of the system which may cause changes
in the output over time based on the memoryless functions parameterised by $\mathbf{A}$ and
$\mathbf{B}$.

<!-- prettier-ignore -->
![Block diagram showing memory and memoryless components](/images/control_system_memory.jpg)
_Systems modelled as a combination of memory elements and memoryless components taken from <em>The
Essentials of Linear State-Space Systems</em> by Aplevich_

## Stability

Assuming that the inputs are bounded and $\mathbf{B}$ has finite entries, the stability of the
system will depend on if the state converges to a particular state over time for any initial state.
This can be determined by solving the set of ODEs which we know results in a linear combination of
exponential functions multiplied by eigenvectors $v_k$ which are themselves a linear combination of
the originally defined states. Since there were $k$ states, there will need to be $k$ independent
eigenvectors for the general solution.

$$
\mathbf{x}(t) = C_1 e^{\lambda_1 t} v_1 + ... + C_k e^{\lambda_k t} v_k
$$

Where $\lambda_k$ are the eigenvalues of $\mathbf{A}$ and . These eigenvectors correspond to motions
that are independent of each other in the system and are known as the modes of system. Clearly from
this equation the system will converge if and only if all of the real parts of the eigenvalues
$\lambda_k$ are negative. Incidentally, these eigenvalues are also the poles of the system in the
[Laplace domain](https://en.wikipedia.org/wiki/Laplace_transform) which we know must be negative for
stability.

## Linearisation

The above formation of linear state-space equations and understanding their stability was predicated
on them being _linear_ systems. However, real systems are always non-linear to some extent and some
moreso than others. Despite there being techniques to handle non-linear systems, they are not nearly
as easy and mature as for linear systems. Fortunately, in many cases, the state of the nonlinear may
only vary over a small region and therefore can be linearised around a point. A common example of a
nonlinear system is a frictionless pendulum modelled as a point mass with mass $m$ attached to a
massless rod of length $L$ that can rotate about a fixed point. It's equation of motion can be
derived as

$$
m \ddot{\theta} = -\frac{mg}{L} \sin\theta
$$

which is clearly a nonlinear.

![A pendulum](/images/simple_pendulum.jpg) _A pendulum taken from <em>Britannica</em>_

If we are modelling the pendulum for only small angles around some $\theta_d$ then we can use the
first order Taylor Series expansion to linearise the system. The Taylor Series expansion around a
point $x = a$ is

$$
f(x) = \sum_{n = 0}^\infty \frac{f^{(n)}(a)}{n!} (x - a)^n
$$

where $f^{(n)}(a)$ is the $n$th derivative of $f$ with respect to $x$ evaluated at $a$. Therefore,
for a first order \(linear\) approximation

$$
f(x) \approx f(a) + \frac{df(a)}{dx} x
$$

Applying this to the pendulum at $\theta_d$,

$$
\ddot{\theta} = -\frac{g}{L} \cos\theta_d \; (\theta - \theta_d)
$$

And if linearising around $\theta_d = 0$ then we get

$$
\ddot{\theta} = -\frac{g}{L} \theta
$$

which is the small angle approximation that many may be familiar with.

In this way, many nonlinear systems can be approximated by linear systems and the same analysis
tools may be used for them. For highly nonlinear systems there are other methods which I may discuss
in a later post.
