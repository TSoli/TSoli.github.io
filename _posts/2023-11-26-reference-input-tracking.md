---
title: Reference Input Tracking
date: 2023-11-26
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
    feedforward control,
  ]
math: true
toc: true
---

<!-- prettier-ignore -->
* TOC
{:toc}

## Adding a Reference Input

<!-- prettier-ignore -->
![A block diagram of a system with state feedback and reference input](/images/statefeedback_w_ref.jpg)
_A block diagram of a system with state feedback and a reference input $\mathbf{r}$_

In [my previous post]({% post_url 2023-11-21-controllability-and-stabilisability %}), I discussed
how to determine if a system is controllable. If it was controllable, I showed this meant its poles
could be arbitrarily chosen allowing the dynamics and stability characteristics of the system to be
manipulated as desired. However, in many cases we want the system to not only stabilise but
stabilise to a desired _reference_ or perhaps even track a desired _reference input_.

![A block diagram of a system with state feedback](/images/statefeedback.jpg) _A block diagram of a
system with state feedback_

To achieve this, we introduce a reference input signal $\mathbf{r}$ which represents the desired
output of the system. Naively, we might try adding this reference input directly to our feedback
control signal so that $\mathbf{u = -Kx + r}$. Therefore we have,

$$
\begin{align*}
  \mathbf{\dot{x}} &= \mathbf{Ax + Bu} \\
  \implies \mathbf{\dot{x}} &= \mathbf{Ax + B(-Kx + r)} \\
  \implies \mathbf{\dot{x}} &= \mathbf{(A - BK)x + Br}
\end{align*}
$$

If we assume the system has been stabilised appropriately with state feedback then at steady state
$\mathbf{\dot{x}} = 0$ by definition. Also noting that $\mathbf{y = Cx + Du},

$$
\begin{align*}
  \mathbf{x}_{ss} &= \mathbf{(A - BK)^{-1} Br} \\
  \mathbf{y}_{ss} &= \mathbf{Cx_{ss} + D(-Kx_{ss} + r)} \\
  \implies \mathbf{y}_{ss} &= \mathbf{((C - DK)(A - BK)^{-1} B + D)r}
\end{align*}
$$

where $$\mathbf{x}_{ss}$$ and $$\mathbf{y}_{ss}$$ are the steady state values for the state and
output, respectively. Clearly, $$\mathbf{y}_{ss} \neq \mathbf{r}$$ in general. But what if we add a
matrix gain $\mathbf{\bar{N}}$ to the reference input so that $\mathbf{u = -Kx + \bar{N}r}$. Then
the above equation becomes

$$
\mathbf{y}_{ss} = \mathbf{((C - DK)(A - BK)^{-1} B + D) \bar{N}r}
$$

Now if we let $\mathbf{y}_{ss} = \mathbf{r}$,

$$
\begin{align*}
  \mathbf{I} &= \mathbf{((C - DK)(A - BK)^{-1} B + D) \bar{N}} \\
  \implies \mathbf{\bar{N}} &= \mathbf{((C - DK)(A - BK)^{-1} B + D)^{-1}}
\end{align*}
$$

Or if $\mathbf{D = 0}$ which is fairly common,

$$
\mathbf{\bar{N}} = \mathbf{(C(A - BK)^{-1} B)^{-1}}
$$

This gives us a system block diagram like this.

<!-- prettier-ignore -->
![A block diagram of a system with state feedback and reference input](/images/statefeedback_w_ref.jpg)
_A block diagram of a system with state feedback and a reference input $\mathbf{r}$_

## Intuition Behind Feedforward Control

As we just saw, the reference input signal is simply solving for the required steady state input to
achieve a desired steady state output. Therefore, it can be thought of as a predictive feedforward
control since it blindly \(without any knowledge of the actual current state\) computes what would
be the required input to keep the system at a desired steady state.

This contrasts with the state feedback control which reacts to the current state to choose an
appropriate response that will stabilise the system. As such, the feedforward control from the
reference input is not only beneficial since it allows an arbitrary reference input but also
because, similarly to derivative control in PID, it generally decreases the response time of the
system by predicting the required input.
