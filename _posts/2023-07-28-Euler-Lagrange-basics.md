---
title: Variational Calculus Basics and the Euler-Lagrange Equation
date: 2023-08-28
categories: [Mechanics, Dynamics]
tags:
  [study, mechanics, dynamics, energy methods, Euler-Lagrange, variational calculus, mathematics]
math: true
---

<!-- prettier-ignore -->
* TOC
{:toc}

## Variational calculus in a nutshell

Most of us probably learned algebra in school and began with some simple manipulations of equations
in order to solve for particular values. Not too long after maybe we started to learn to sketch
these equations in order to be able to visualise the mathematical relationships we were learning
about better. Towards the end of highschool and beginning of uni we then became interested in
different aspects of these equations like their slope or instantaneous changes or the area
underneath them. This is known as differential calculus and one of the most common ways we use it is
to find maxima and minima (and sometimes other stationary points) known as optimisation.

Variational calculus is a fairly simple extension of this where instead of having a function of a
variable that we would like to find a minimum or maximum value for, we have a function of a function
(known as a functional) and we want to find input functions which produce maximum or minimum values
(collectively known as extrema or stationary functions).

So what are some problems that this allows us to solve? Well actually there are
[a ton of applications](https://en.wikipedia.org/wiki/Calculus_of_variations#Applications) in areas
such as classical mechanics, optics, control and quantum mechanics. In a general sense, we are
solving for optimal paths. If you're a computer scientist or software engineer you are probably
familiar with shortest path algorithms like
[Dijkstra's algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm) and I would say this is
in many ways the continuous and analytical counterpart.

## A trivial example

To begin, let's look at a common trivial example for applying variational calculus which is to find
the function for the minimum distance between two points in the Cartesian plane, $p_1=  (x_1, y_1)$
and $p_2 = (x_2, y_2)$.

<!-- prettier-ignore -->
![](/images/variational_calc/shortest_path.jpg) _A 2D graph showing an arbitrary path $s$ and
optimal path $$s_{min}$$ between two arbitrary points $$p_1$$ and $$p_2$$_

Firstly, we can imagine breaking an arbitrary line up into infinitesimal segments of length $ds$. At
this scale the curvature can be considered as 0 and as such we have infinitesimal straight segments.
Clearly, by Pythagoras' theorem, $ds = \sqrt{dx^2 + dy^2}$.

![](/images/variational_calc/segment.jpg) _An infinitesimal segment of the line, $ds$_

Now we know that the length of the line is simply the sum of each of the inifinitesmal parts and as
their length approaches 0 we get the integral of each of their infinitesimal parts. Therefore we
have,

$$
s = \int_{p_1}^{p_2} ds = \int_{p_1}^{p_2} \sqrt{dx^2 + dy^2}
$$

And with some simple refactoring,

$$
s = \int_{x_1}^{x_2} \sqrt{1 + \left(\frac{dy}{dx}\right)^2} dx
$$

Here we have represented $s$ as a function of $y(x)$ and as such s is known as a _functional_. In
order to find the shortest path, we must find the function $y(x)$ which minimises the functional
$s$. So firstly, we can represent an arbitrary path $\bar{y}(x)$ between $p_1$ and $p_2$ as the
optimal path (which we don't yet know the solution for) plus a function that varies from it
multiplied by a scalar, $\epsilon \eta(x)$. That is,

$$
\begin{align*}
  \bar{y}(x) &= y(x) + \epsilon \eta(x) \\
  \implies \frac{d \bar{y}}{dx} &= \frac{dy}{dx} + \epsilon \frac{d \eta}{dx}
\end{align*}
$$

Clearly, as $\epsilon \rightarrow 0$ we get the optimal path, $y(x)$. So we can write a general
equation for our functional, $s$ using our general function $\bar{y}(x)$ and then, just like
differential calculus, if we differentiate it with respect to $\epsilon$ we expect extrema when this
is equal to 0. Furthermore, if we evaluate this derivative at $\epsilon = 0$ we know that it will be
a minimum. So,

$$
\begin{gather}
  \frac{ds}{d\epsilon} \bigg\rvert_{\epsilon = 0} = 0 \\
  \implies \frac{d}{d\epsilon} \bigg\rvert_{\epsilon = 0} \; \int_{x_1}^{x^2} \sqrt{1 +
  \left(\frac{d\bar{y}}{dx}\right)^2} \; dx = 0
\end{gather}
$$

It is important to note here that for a given problem, $y(x)$, $\bar{y}(x)$ and $\eta(x)$ are all
fixed and only $\epsilon$ varies which is why this is _not_ a partial derivative with respect to
$\epsilon$. Now we can move the derivative into the integral because of
[Leibniz rule](https://en.wikipedia.org/wiki/Leibniz_integral_rule)

$$
\int_{x_1}^{x_2} \frac{\partial}{\partial\epsilon} \; \sqrt{1 + \left(\frac{d\bar{y}}{dx} \right)} \bigg\rvert_{\epsilon = 0} \; dx = 0
$$

Now we can apply the chain rule to solve this. First to make it easier I will use the notation,

$$
\begin{align*}
  \frac{\bar{y}}{dx} &= \dot{\bar{y}} &\text{(and similar for $\dot{y}$ and $\dot{\eta}$)} \\
  F(\dot{\bar{y}}) &= \sqrt{1 + \left( \dot{\bar{y}} \right)^2 }
\end{align*}
$$

And now by the chain rule,

$$
\begin{align*}
  \frac{\partial F}{\partial \epsilon} &= \frac{\partial F}{\partial \dot{\bar{y}}} \; \frac{\partial \dot{\bar{y}}}{\partial \epsilon} \\
  \implies \frac{\partial F}{\partial \dot{\bar{y}}} &= \frac{1}{2} (1 + \dot{\bar{y}})^{ - \frac{1}{2} } \\
  \implies \frac{\partial \dot{\bar{y}}}{\partial \epsilon} &= \dot{\eta} \\
  \implies \frac{\partial F}{\partial \epsilon} &= \left( \frac{1}{2} (1 + \dot{\bar{y}})^{ - \frac{1}{2} } \right) \dot{\eta} \\
  \implies \frac{\partial F}{\partial \epsilon} &= \left( \frac{1}{2} ( 1 + \dot{y} + \epsilon \dot{\eta} )^{ - \frac{1}{2}} \right) \dot{\eta} \quad \text{(substituting from the equation for $\dot{\bar{y}}$)}
\end{align*}
$$

Now evaluating at $\epsilon = 0$ we get,

$$
\frac{\partial F}{\partial \epsilon} \bigg\rvert_{\epsilon = 0} = \left( \frac{1}{2} ( 1 + \dot{y} )^{ - \frac{1}{2}} \right) \dot{\eta}
$$

And now we can integrate it with respect to $x$ between from $p_1$ to $p_2$ and set this to 0,

$$
0 = \int_{x_1}^{x_2} \left( \frac{1}{2} ( 1 + \dot{y} )^{ - \frac{1}{2}} \right) \dot{\eta} \; dx
$$

The above equation is now the first variation in its weak form since it includes $\dot{\eta}$.
Fortunately, we can apply integration by parts to put it into its strong form which only includes
$\eta(x)$,

$$
0 = \left( \frac{1}{2} ( 1 + \dot{y} )^{ - \frac{1}{2}} \right) \; \eta \bigg\rvert_{x_1}^{x_2} - \int_{x_1}^{x_2} - \frac{1}{4} \eta \ddot{y} (1 + \dot{y})^{ - \frac{1}{2}} \; dx
$$

Clearly, since the path $\dot{\bar{y}}$ still begins and ends at the points $p_1$ and $p_2$
\(remember $\bar{y}$ represents an arbitrary path from $p_1$ to $p_2$\), the variation, $\eta(x)$ at
these points must be 0 \(such that $\bar{y}(x_1) = y(x_1) = y_1$ and the same for $x_2$ and $y_2$\).
As a result, the first part of our expression (outside the integral) vanishes when we evaluate it.
Furthermore,
[the fundamental lemma of these calculus of variations](https://en.wikipedia.org/wiki/Fundamental_lemma_of_the_calculus_of_variations)
states that since $\eta(x)$ is arbitrary (remember it must represent any potential path from $p_1$
to $p_2$) then the function that is multiplying it in the integral must also be 0
$\forall \; p_1 \leq x \leq p_2$. Therefore,

$$
\begin{align*}
  0 &= \ddot{y} (1 + \dot{y})^{ - \frac{1}{2}} \\
  \implies 0 &= \ddot{y} \\
  \implies 0 &= \dot{y} + C_1 \\
  \implies 0 &= y + C_1 x + C_2
  \implies y &= C_1 x + C_2
\end{align*}
$$

And this is the equation for a straight line which we know is the shortest path between two points
in Euclidean space. Hooray! Now we can simply plug in the known endpoints at $p_1$ and $p_2$ to get
the exact straight line that is the shortest path between $p_1$ and $p_2$ \(this is some simple
algebra that you should be familiar with so I will just give the result\).

$$
\begin{gather*}
  C_1 = \frac{y_2 - y_1}{x_2 - x_1} \\
  C_2 = y_1 - C_1 x_1
\end{gather*}
$$

Ok... but that was a lot of work. Can we generalise it somehow?

## The Euler-Lagrange Equation

Well, it turns out the answer is yes! In general, our functional can be represented,

$$
S[y] = \int_{x_1}^{x_2} F(x, y(x), \dot{y}(x)) \; dx
$$

$F$ may not always be a function of all three directly \(as was the case in the example above\) but
for many problems it can be \(e.g problems like the brachistocrone, catenary and others
[here](https://www.open.edu/openlearn/mod/resource/view.php?id=72745) for some examples\).
