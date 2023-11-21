---
title: System Controllability and Stabilisability
date: 2023-11-21
categories: [Mechanics, Control]
tags: [study, mechanics, dynamics, mathematics, control, state space, linear systems]
math: true
toc: true
---

<!-- prettier-ignore -->
* TOC
{:toc}

## State Feedback

In [my previous post]({% post_url 2023-10-29-state-space %}), I discussed how many systems can be
modelled using linear state space equations of the form $\mathbf{\dot{x} = Ax + Bu}\;$ where
$\mathbf{x}$ is the state of the system, $\mathbf{u}$ are the inputs of the system and $\mathbf{A}$
and $\mathbf{B}$ parameterise the system. However, in many real scenarios, it is not enough for us
to just have a system of the model. Often we want to control the system into a given state that is
useful for our purposes or at the very least, ensure it remains stable. State feedback is a popular
method that allows us to do just this.

<!-- prettier-ignore -->
![A block diagram of a system with state feedback](/images/statefeedback.jpg)
_A block diagram of a system with state feedback_

For linear systems, we can use a very simple control law, $\mathbf{u = Kx}$, and remarkably this
allows us to choose _arbitrary_ closed loop system poles and as a result dynamics of the system. How
is this so?

## State Space System Representations

The state space representation of a system is not unique. Despite this, we will see that some
choices of states lead to a clearer understanding of certain parts of the system dynamics. To begin,
consider an arbitrary state space system described by

$$
\begin{align*}
  \mathbf{\dot{x}} &= \mathbf{Fx + Gu} \\
  \mathbf{y} &= \mathbf{Hx + Ju}
\end{align*}
$$

Now let $\mathbf{x = Tz}$ for some other choice of state $\mathbf{z}$ and a nonsingular matrix
$\mathbf{T}$. Substituting this into our state space equations from before and rearranging,

$$
\begin{align*}
  \mathbf{T\dot{z}} &= \mathbf{FTz + Gu} \\
  \implies \mathbf{\dot{z}} &= \mathbf{T^{-1}FTz + T^{-1}Gu} \\
  \mathbf{y} &= \mathbf{HTz + Ju}
\end{align*}
$$

If

$$
\begin{align*}
  \mathbf{A} &= \mathbf{T^{-1}FT} \\
  \mathbf{B} &= \mathbf{T^{-1}G} \\
  \mathbf{C} &= \mathbf{HT}
\end{align*}
$$

then

$$
\begin{align*}
  \mathbf{\dot{z}} &= \mathbf{Az + Bu} \\
  \mathbf{y} &= \mathbf{Cx}
\end{align*}
$$

which is still a linear state space equation for the system. Therefore, the system's state space
representation is not unique.

## Deriving the Transfer Function

State space equations can also be represented in the Laplace domain. Begin by recalling that the
derivative in the time domain is equivalent to multiplying by $s$ in the Laplace domain. Therefore,
rewriting the state space equations we get.

$$
\begin{align*}
  s\mathbf{X} &= \mathbf{AX + BU} \\
  \mathbf{Y} &= \mathbf{CX + DU}
\end{align*}
$$

Rearranging this we get

$$
\begin{align*}
  \mathbf{X} &= (s\mathbf{I - A})^{-1} \mathbf{BU} \\
  \implies \mathbf{Y} &= (\mathbf{C}(s\mathbf{I - A})^{-1} \mathbf{B} + \mathbf{D}) \mathbf{U}
\end{align*}
$$

Therefore, the transfer function $\mathbf{H}(s)$ is,

$$
\mathbf{H}(s) = \mathbf{C}(s\mathbf{I - A})^{-1} \mathbf{B} + \mathbf{D}
$$

Some other useful relationships are that the characteristic equation can be found with

$$
\det(s\mathbf{I - A}) = 0
$$

and the zeros can be found with

$$
\det \begin{pmatrix}
  s\mathbf{I - A} & -\mathbf{B} \\
  \mathbf{C}  & \mathbf{D}
\end{pmatrix}
$$

These are a little harder to show so for the sake of brevity I will not derive them but for more
details see chapter 7 of _Feedback Control of Dynamic Systems_ by Franklin and Powell or you can do
some searching online.

## Controllability Canonical Form

The controllability canonical form of a system can be expressed with,

$$
\begin{align*}
  \mathbf{A}_c &= \begin{pmatrix}
    -a_1 & -a_2 & \dots & -a_{n-1} & -a_n \\
    1 & 0 & \dots & 0 & 0 \\
    0 & 1 & \dots & 0 & 0 \\
    \vdots & \vdots & \ddots & \vdots & \vdots \\
    0 & 0 & \dots & 1 & 0
  \end{pmatrix}, \quad
  &\mathbf{B}_c = \begin{pmatrix} 1 \\ 0 \\ \vdots \\ 0 \end{pmatrix}
  \\
  \mathbf{C}_c &= \begin{pmatrix}
    b_1 & b_2 & \dots & b_n
  \end{pmatrix}, \quad
  &\mathbf{D}_c = \mathbf{0}
\end{align*}
$$

where the transfer function of the system is

$$
H(s) = \frac{b_1 s^{n-1} + b_2 s^{n-2} + \dots + b_n}{s^n + a_1 s^{n-1} + \dots + a_n}
$$

So this form of the system is very useful because it allows us to easily identify the dynamics of
the system using the poles and zeros.

Now consider the system with state feedback such that $\mathbf{u = Kx}$ \(note some resources use
$\mathbf{u = -Kx}$ which is equivalent except that the signs of each element of $\mathbf{K}$ would
be negated\). Then the system could be rewritten,

$$
\mathbf{\dot{x}} = \mathbf{Ax + BKx} = (\mathbf{A + BK})\mathbf{x}
$$

So the effective $\mathbf{A}$ matrix of the system is $\mathbf{A + BK}$. If the system was initially
in control canonical form then,

$$
\begin{align*}
  \mathbf{BK} &= \begin{pmatrix}
    k_1 & k_2 & \dots & k_n \\
    0   & 0   & \dots & 0 \\
    \vdots & \vdots & \ddots & \vdots \\
    0   & 0   & \dots & 0
  \end{pmatrix} \\
  \implies \mathbf{A + BK} &= \begin{pmatrix}
    -a_1 + k_1 & -a_2 + k_2 & \dots & -a_{n-1} + k_{n-1} & -a_n + k_n \\
    1          & 0          & \dots & 0                  & 0 \\
    \vdots     & \vdots     & \ddots & \vdots            & 0 \\
    0          & 0          & \dots & 1                  & 0
  \end{pmatrix}
\end{align*}
$$

And so we can arbitrarily choose the poles of our system by choosing $\mathbf{K}$ such that the
coefficients of the desired characteristic equation match the terms in the resultant controllability
matrix for the system. The corollary is that if we can represent the system in control canonical
form then the system poles can be chosen arbitrarily therefore so can the system's dynamics. If this
is the case then we say the system is _controllable_.

## The Controllability Matrix

So how can we show if it is possible to represent the system in this controllability canonical form?
Well, let's take our arbitrary system from before and try to transform it into controllability form.
We already saw,

$$
\begin{align*}
  \mathbf{A}_c &= \mathbf{T^{-1}FT} \\
  \implies \mathbf{A}_c \mathbf{T^{-1}} &= \mathbf{T^{-1}F}
\end{align*}
$$

Now if the rows of $\mathbf{T^{-1}}$ are denoted as $\mathbf{t_1,\, t_2, \dots,\, t_n}$ then
rewriting the previous equation,

$$
\begin{pmatrix}
  -a_1 & \dots & -a_n \\
  1    & \dots & 0    \\
  \vdots & \ddots & \vdots \\
  0    & \dots & 0
\end{pmatrix}
\begin{pmatrix} \mathbf{t_1} \\ \mathbf{t_2} \\ \vdots \\ \mathbf{t_n} \end{pmatrix}
= \begin{pmatrix} \mathbf{t_1 F} \\ \mathbf{t_2 F} \\ \vdots \\ \mathbf{t_n F} \end{pmatrix}
$$

From this, we can see that

$$
\begin{align*}
  \mathbf{t_{n-1}} &= \mathbf{t_n F} \\
  \implies \mathbf{t_{n-2}} &= \mathbf{t_{n-1} F} = \mathbf{t_n F^2} \\
  \implies \mathbf{t_{n-k}} &= \mathbf{t_n F^k}
\end{align*}
$$

We also know that

$$
\begin{align*}
  \mathbf{T^{-1}G} &= \mathbf{B}_c \\
  \implies \begin{pmatrix} \mathbf{t_1 G} \\ \vdots \\ \mathbf{t_n G} \end{pmatrix}
  &= \begin{pmatrix} 0 \\ \vdots \\ 1 \end{pmatrix}
\end{align*}
$$

Combining these

$$
\begin{align*}
  \begin{pmatrix} \mathbf{t_n G} & \mathbf{t_{n-1} G} & \dots & \mathbf{t_1 G} \end{pmatrix}
  &= \begin{pmatrix} 0 & 0 & \dots & 1 \end{pmatrix} \\
  \implies \mathbf{t_n} \begin{pmatrix}
    \mathbf{G} & \mathbf{FG} & \dots & \mathbf{F^{n - 1} G}
  \end{pmatrix}
  &= \begin{pmatrix} 0 & 0 & \dots & 1 \end{pmatrix} \\
  \implies \mathbf{t_3}
  = \begin{pmatrix} 0 & 0 & \dots & 1 \end{pmatrix} \mathcal{C}^{-1}
\end{align*}
$$

where
$\mathcal{C} = \begin{pmatrix} \mathbf{G} & \mathbf{FG} & \dots & \mathbf{F^{n-1}G} \end{pmatrix}$
is the controllability matrix. Therefore, if $\mathcal{C}$ is invertible then we can solve for
$\mathbf{t_n}$ which then allows us to solve for each of the additional rows of $\mathbf{T^{-1}}$.
Using this $\mathbf{T^{-1}}$ we can then transform the system into control canonical form and then
move the poles of the system arbitrarily with an appropriate $\mathbf{K}$.

So, the practical steps of assessing controllability are simply to construct the controllability
matrix
$\mathcal{C} = \begin{pmatrix} \mathbf{G} & \mathbf{FG} & \dots & \mathbf{F^{n-1}G} \end{pmatrix}$
and then check if it is invertible. A practical method for checking if it is invertible is to check
if it has **rank**$(\mathcal{C}) = n$ where $n$ is the number of states of the system. Many software
packages make this computation easy but if for whatever reason you need to do it by hand you can
check the rank by converting the matrix to
[row echelon form](https://en.wikipedia.org/wiki/Row_echelon_form) using row operations and count
the number of non-zero rows.

## An Intuitive Understanding of Controllability

If a system is controllable, then it is possible to control the dynamics of every mode of the
system. In other words, the speed at which each of the modes stabilises can be set arbitrarily with
an appropriate $\mathbf{K}$ used in state feedback. On the other hand, if a system is not
controllable, we cannot make these guarantees for all modes. For example, if the accelerator of a
car provides the input to the system and we had sensors to measure all the states perfectly, we
could control the horizontal motion of the car. However, the vertical motion of the car would be
independent of this input and so we would not be able to control this vertical motion using only the
accelerator. In this case, if only the horizontal position and velocity are considered as states,
the system would be controllable however, if the vertical motion also needs to be considered then
the system would not be controllable.

## Stabilisability

Despite this, the car would likely be fitted with some suspension which could be modelled as a
spring and damper. This limits its vertical motion from continuing indefinitely after a disturbance
such as a speed bump. In this case, even though we cannot directly control the car's vertical
motion, we can at least say that this uncontrollable mode is stable and as such we could still say
that the system is stabilisable. More generally, if all of the uncontrollable modes of a system are
naturally stable then the system is said to be _stabilisable_. It does not matter whether the modes
that we can control are naturally stable or not since we can control their dynamics arbitrarily with
state feedback as shown before to make them stable.

<!-- prettier-ignore -->
![A diagram of a car modelled with springs and dampers at each wheel](/images/car_with_suspension.jpg)
_A car with suspension modelled using springs and dampers at each wheel_

But how do we know which modes of the system are controllable? The controllability matrix rank just
tells us if the system is controllable but not which modes are not controllable. Therefore, a common
way to check which modes are uncontrollable is to use the
[PBH test](https://en.wikipedia.org/wiki/Hautus_lemma) which tells us not only if the system is
controllable but also which modes are controllable. The PBH test says that a mode is controllable if
and only if

$$
\text{rank}\left(\begin{array}{c:c}
  \lambda \mathbf{I - A} & \mathbf{B}
\end{array} \right) = n
$$

for the mode associated with the eigenvalue $\lambda$. Therefore, a system is stabilisable if and
only if the above is true for all $\Re(\lambda) \geq 0$. A proof for this can be found
[here](https://www.control.utoronto.ca/people/profs/kwong/ece410/2008/notes/chap2.pdf).

## Practical Limitations

<!-- prettier-ignore -->
![A VW Golf with a Lamborghini V10 Engine in the boot](/images/vw_golf_with_v10.jpg)
_A VW Golf with a Lamborghini V10 engine to make it go zoom_

While in theory if a given mode is controllable it is possible to control its dynamics arbitrarily
in practice this is often limited by the input. Going back to the example of the car, there is
obviously a limit to the amount of force that can be applied to the car by the engine and therefore
the accelerator. This limit means that we can control the dynamics of the car up to a point but my
VW golf is still not ever going to go from 0-100km/h in one second. Similarly, some modes may be
weakly controllable making it impractical to control them. For example, accelerating and then
decelerating may induce a small amount of rocking in the car however the effect would be small and
it would be impractical in many cases to implement control of this mode just from the brakes and
accelerator.
