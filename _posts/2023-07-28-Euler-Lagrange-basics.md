---
title: Variational Calculus Basics and the Euler-Lagrange Equation
date: 2023-07-28
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
optimal path (which we don't yet know the solution for) plus a function that varies from it \(except
at the endpoints\) multiplied by a small scalar, $\epsilon \eta(x)$. That is,

$$
\begin{align*}
  \bar{y}(x) &= y(x) + \epsilon \eta(x), \quad \text{where } \eta(x_1) = \eta(x_2) = 0 \\
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
  \implies 0 &= y + C_1 x + C_2 \\
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
[here](https://www.open.edu/openlearn/mod/resource/view.php?id=72745) for some examples\). Assuming
this, we can represent our functional including the variation, $\epsilon \eta(x)$ by making the same
substitutions as before \(i.e $\bar{y} = \epsilon \eta(x)$ and
$\dot{\bar{y}} = \epsilon \eta(x)\). Now, for a fixed $y$ and $\eta$, $F$ is just a function of
$\epsilon$ and since $y$ was defined as the stationary path, clearly $S[\bar{y}]$ is stationary at
$\epsilon = 0$. That is,

$$
\frac{dF}{d\epsilon} \; \bigg\rvert_{\epsilon = 0} = \frac{d}{d\epsilon} \int_{x_1}^{x_2}
  F(x, \bar{y}(x), \dot{\bar{y}}(x)) \; dx \; \bigg\rvert_{\epsilon = 0} = 0
$$

Now we can apply a similar process to above with Leibniz's rule,

$$
0 = \int_{x_1}^{x_2} \frac{d}{d\epsilon} F(x, \bar{y}(x), \dot{\bar{y}}(x)) \; dx
$$

And then noting that $x$ does not depend on $\epsilon$, applying the chain rule,

$$
\begin{align*}
  \frac{dF}{d\epsilon} \bigg\rvert_{\epsilon = 0} &= \left( \frac{\partial F}{\partial \bar{y}}
    \frac{d\bar{y}}{d\epsilon} + \frac{\partial F}{\partial \dot{\bar{y}}}
    \frac{d\dot{\bar{y}}}{d\epsilon} \right) \; \bigg\rvert_{\epsilon = 0} \\
  \implies \frac{dF}{d\epsilon} \bigg\rvert_{\epsilon = 0} &= \left(
    \frac{\partial F}{\partial \bar{y}} \eta + \frac{\partial F}{\partial \dot{\bar{y}}} \dot{\eta}
    \right) \; \bigg\rvert_{\epsilon = 0}
\end{align*}
$$

Now as $\epsilon \rightarrow 0$, $\bar{y} \rightarrow y$ and $\dot{\bar{y}} \rightarrow \dot{y}$ so,

$$
\frac{dF}{d\epsilon} \bigg\rvert_{\epsilon = 0} =
  \frac{\partial F}{\partial y} \eta + \frac{\partial F}{\partial \dot{y}} \dot{\eta}
$$

Now substituting that back into the functional expression we get the first variation of $F$ in its
weak form. Then, integrating by parts,

$$
0 = \frac{\partial F}{\partial \dot{y}} \eta \; \bigg\rvert_{x_1}^{x_2} +
  \int_{x_1}^{x_2} \left( \frac{\partial F}{\partial y} - \frac{d}{dx} \left(
  \frac{\partial F}{\partial \dot{y}} \right) \right) \eta \; dx
$$

We know that $\eta(x_1) = \eta(x_2) = 0$ at the boundaries so clearly the first part of the
expression vanishes. Then, because $\eta(x)$ is arbitrary, the expression it is multiplying in the
integral must be 0. That is,

$$
\frac{d}{dx} \left( \frac{\partial F}{\partial \dot{y}} \right) - \frac{\partial F}{\partial y} = 0
$$

And this is the Euler-Lagrange equation which is much easier to use than following the whole process
above for each problem.

## Lagrangian Mechanics

One set of problems that this result is very useful for is in classical mechanics.
[The stationary-action principle](https://en.wikipedia.org/wiki/Stationary-action_principle) \(also
known as the principle of least action or
[Hamilton's principle](https://en.wikipedia.org/wiki/Hamilton%27s_principle)\) states that
trajectories of objects are stationary solutions to the action functional which is defined,

$$
S[\mathbf{q}(t)] = \int_{t_1}^{t_2} L(\mathbf{q}(t), \dot{\mathbf{q}}(t), t) \; dt
$$

where, $\mathbf{q}(t)$ is a vector of independent
[generalised coordinates](https://en.wikipedia.org/wiki/Generalized_coordinates) for the system
\(i.e the minimum number of coordinates required to describe the configuration of the system\) and
$L$ is the Lagrangian defined as,

$$
L = T - V
$$

where, $T$ is the kinetic energy and $V$ is the potential energy, which are functions of
$\dot{\mathbf{q}}$ and $\mathbf{q}$, respectively. I found out the reason for this is
[related to quantum mechanics \(see page 6/7\)](https://scholar.harvard.edu/files/david-morin/files/cmchap6.pdf)
which I have not studied so I won't attempt to explain it here. Anyway, applying our result from
before,

$$
\frac{d}{dt} \left( \frac{\partial L}{\partial \dot{\mathbf{q}}} \right) - \frac{\partial L}{\partial \mathbf{q}} = 0
$$

In fact, if we apply this equation to the Cartesian coordinates, $\mathbf{x} = (x, y, z)$, we get,

$$
m\ddot{\mathbf{x}} = -\nabla V(x, y, z)
$$

And $-\nabla V = \mathbf{F}$. In other words,

$$
\mathbf{F} = m\mathbf{a}
$$

which, of course, is
[Newton's second law of motion](https://en.wikipedia.org/wiki/Newton%27s_laws_of_motion). Now, $V$
only incorporated potential energy which corresponds with
[conservative forces](https://en.wikipedia.org/wiki/Conservative_force), which by defintion are a
function of their position only. In many real scenarios, we also need to consider non-conservative
forces such as friction, drag or external forces on our system that can do work. Collectively, these
non-conservative forces can be represented as $\mathbf{Q}$ and since we just showed that the
Euler-Lagrange equation works out to Newton's law, we can add these non-conservative forces into the
Euler-Lagrange equation,

$$
\frac{d}{dt} \left( \frac{\partial L}{\partial \dot{\mathbf{q}}} \right) - \frac{\partial L}{\partial \mathbf{q}} = \mathbf{Q_i}
$$

And this is the form that we can use to easily solve many mechanics problems while still considering
non-conservative forces on our system.

## Why not use Newtonian mechanics?

This seems like quite a roundabout way of getting to Newton's equations which may arguably be easier
to use for simple one dimensional problems so what was the point? Well the big win here is that for
problems that have multiple degrees of freedom \(that is they require many generalised coordinates
to fully represent the configuration of the system\), Lagrange's equation is much simpler to work
with. Consider that using Lagrange's method we are free to pick whatever set of generalised
coordinates are natural for the problem, we then only have to calculate energies \(except for the
non-conservative forces\) which are scalar values making it easier to avoid mistakes with direction.
The equations of motion can then be derived analytically without much difficult geometry. As a
result, for these more complex systems, Lagrangian mechanics are a much more practical way of
solving problems.

## Main takeaways and further reading

This was quite an interesting deep dive for me. I hadn't learned about variational calculus before
and I think it's a really cool concept. It seems to be quite beautifully applied to many natural
phenomena so perhaps it is not the last of it I will come across. In saying that, if I made any
mistakes or there are any super interesting insights I missed I would love to hear about it in the
comments below. There were some things I wanted to investigate more that I cam across like
[Hamiltonian mechanics](https://en.wikipedia.org/wiki/Hamiltonian_mechanics) and
[D'Alembert's principle](https://en.wikipedia.org/wiki/D%27Alembert%27s_principle) but I simply did
not have the time. Maybe that can be a topic for a future post. Either way I'll leave some
references I found particularly useful for this topic below.

- [A video on variational calculus and the Lagrange Equation](https://www.youtube.com/watch?v=VCHFCXgYdvY)
- [A physics teaching resource from Harvard](https://scholar.harvard.edu/files/david-morin/files/cmchap6.pdf)
- [A teaching resource on variational calculus](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwiNt_eki7iAAxWYnFYBHeQ_C9YQFnoECBAQAQ&url=https%3A%2F%2Fwww.open.edu%2Fopenlearn%2Fmod%2Fresource%2Fview.php%3Fid%3D72745&usg=AOvVaw0v7lFBPU5E-si5LrRcpMG1&opi=89978449)
- [A nice derivation of Lagrange's equation from Brown University](https://www.brown.edu/Departments/Engineering/Courses/En137/Lagrange.pdf)
- Course of Theoretical Physics vol. 1 by Landau and Lifshitz
