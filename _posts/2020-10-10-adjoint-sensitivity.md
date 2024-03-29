---
layout: post
title: Adjoint sensitivities equations
date: 2020-02-10 11:12:00-0400
description: A derivation of the adjoing sensitivities equations for the neural ODE paper
comments: true
---

This post will be the first of several on neural ODEs. Specifically, we will be going through the paper **Neural Ordinary Differential Equations** by Ricky T. Q. Chen et. al. (2018) and available here: <a href="https://arxiv.org/pdf/1806.07366.pdf">https://arxiv.org/pdf/1806.07366.pdf</a>.

The story begins with the following problem. Suppose that we have some continuous process that can be described by a time-dependent state $$x(t)$$. We now assume that this state obeys  ** deterministic ** dynamics, specified by the following ordinary differential equation (ODE):

$$ h(x, \dot{x}, \theta; t) = 0 $$

with initial condition

$$ g(x_0, \theta) = 0 $$

where $\theta$ represent model parameters that will be learned. In usual machine learning, the theta parametrize some class of functions, like the mean and standard deviation of a Gaussian for example. Here, these parameters parametrize a differential equation. As a quick example, imagine parametrizing a class of functions to represent exponential decay. We might write something like $$ f(x(t); \theta) = e^{-\theta t} $$. This can similarly be represented via a class of differential equations $$ h(x, \dot{x}, \theta) = \dot{x} - b x $$ with initial condition $$ g(x_0) = 1 $$. Great, so nothing about our procedure is too different. We now suppose that we have some data about the observed states $$ x_\text{obs}(t) $$ and wish to "fit" the data to our class of parametrized ODEs. We can do so in the usual way, by defining a loss function. Only this time, our loss function needs to be a loss *functional*. We write the following, totally analogous to how we write the action-lagrangian relationship

$$ L(t_f, t_i) = \int_{t_i}^{t_f} \mathcal{L}(x(t), \dot{x}(t), \theta; t) dt $$

This can be thought of as summing the outputs of loss functions evaluated at every possible time in the interval from $$t_i$$ to $$t_f$$. To fit the model, we wish to minimize this over $\theta$, hence we write the model learning objective:

$$ \theta = \underset{\theta}{\text{arg min}} \int_{t_i}^{t_f} \mathcal{L}(x(t), \dot{x}(t), \theta; t) dt $$

subject to the constraints

$$ h(x, \dot{x}, \theta; t) = 0 $$

and initial condition

$$ g(x_0, \theta) = 0. $$

That's the goal. Note, this differs from the paper. There, they assume that the loss is a function of *only* the final state $$ x(t_f) $$. This is more general. Ok, so now that we have this, we do the usual step for constrained optimization; introduce Lagrange multipliers. We thus can get away with only one minimization of the following action

$$ L' = \int_{t_i}^{t_f} \left[ \mathcal{L}(x(t), \dot{x}(t), \theta; t) + \lambda^T(t) h(x, \dot{x}, \theta; t) \right] dt + \mu^T g(x_0, \theta). $$

Note, the Lagrange multiplier $$ \lambda^T(t) $$ is a vector-valued function of time. To minimize over $$ \theta $$ we just take the derivative. Until now, I have tried to keep the notation a little cleaner that it should be. The function $$ x(t) $$ is really $$ x(\theta, t) $$, since clearly even in our simple exponential example, $$ x $$ may contain $$ \theta $$ dependence. Great, with that out of the way we now write this long equation

$$
\frac{dL'}{d\theta} = \int_{t_i}^{t_f}
\left[
  \frac{\partial \mathcal{L} }{\partial \theta} +
  \left(
    \frac{\partial \mathcal{L}}{\partial x} +
    \lambda^T \left(
      \frac{\partial h}{\partial x} +
      \frac{\partial h}{\partial \dot{x}}\frac{d}{dt}
      \right )
  \right)\frac{d x}{d \theta}
  + \lambda^T\frac{\partial h}{\partial \theta}
\right] dt +
\mu^T \frac{d g}{d \theta}
$$

Performing the usual integration by parts to move the time derivative off of $$ dx/d\theta$$ and to expand the differential of $$ g $$ we get

\begin{equation}
\label{eq:long lagrange1}
\frac{dL'}{d\theta} = \int_{t_i}^{t_f}
\left[
  \frac{\partial \mathcal{L} }{\partial \theta} +
  \left(
    \frac{\partial \mathcal{L}}{\partial x} +
    \lambda^T \left(
      \frac{\partial h}{\partial x} -
      \frac{d}{dt} \frac{\partial h}{\partial \dot{x}}
      \right ) -
  \frac{d \lambda^T }{dt} \frac{\partial h}{\partial \dot{x}}
  \right)\frac{d x}{d \theta} + \lambda^T\frac{\partial h}{\partial \theta}
\right] dt
\end{equation}

\begin{equation}
\label{eq:long lagrange2}
+\left[ \lambda^T(t) \frac{\partial h}{\partial \dot{x}} \frac{dx}{d\theta}\right]_{t_i}^{t_f} +
\mu^T \frac{\partial g}{\partial x_0} \frac{d x_0}{d\theta} +
\mu^T \frac{\partial g}{\partial \theta}
\end{equation}

Now we solve for the multipliers. Since the value $$ x(\theta, t) $$ is the solution of an ODE, we would like to avoid having to solve it. We shall then choose our function $$ \lambda(t) $$ in such a way as to minimize doing so. This procedure is similar to setting the boundary values of a Green's function via constraints on its variations (see Zangwill E&M). We will make the following choices

1. Set $$ \lambda(t_f) = 0 $$. This takes care of the boundary at the end time $$ t_f $$.
2. Set $$ \mu^T \frac{\partial g}{\partial x_0}\frac{d x_0}{d \theta} = \lambda^T(t_i) \left[\frac{\partial h}{\partial \dot{x}}\frac{d x}{d\theta}\right]_{t_i} $$. This takes care of the boundary at the start time $$ t_i $$.
3. Set $$ (...)=0 $$, where $$\int (...) \frac{dx}{d\theta} dt $$ is the bulk equation of motion. appearing in Eq. \ref{eq:long lagrange1}.

We now have the solution

$$
\frac{dL'}{d\theta} = \int_{t_i}^{t_f} \left[\frac{\partial \mathcal{L}}{\partial \theta} +
\lambda^T \frac{\partial h}{\partial \theta} \right] dt
+ \mu^T \left. \frac{\partial g}{\partial \theta} \right|_{t_i}
$$

$$ \mu^T = \left.
\lambda^T \frac{\partial h}{\partial \dot{x}} \left(\frac{\partial g}{\partial x_0}\right)^{-1}
\right |_{t_i}
$$

$$
\frac{\partial \mathcal{L}}{\partial x} +
\lambda^T \left(
  \frac{\partial h}{\partial x} -
  \frac{d}{dt} \frac{\partial h}{\partial \dot{x}}
  \right ) -
\frac{d \lambda^T }{dt} \frac{\partial h}{\partial \dot{x}} = 0
$$

The first equation is the answer we're looking for, the second two are the auxiliary equations due to the Lagrange constraint. Alright, now let's make things simple! We assume the following first-order (everything we just did can be redone for 2nd order, but that's too much work to do for something we won't use right now) ODE

$$
\frac{dx}{dt} = f(x, \dot{x}, \theta; t) \implies h = \dot{x} - f.
$$

We now have the simple answers $$\partial_{\dot{x}} h = 1$$, $$\partial_x h = -\partial_x f$$, and $$ \partial_\theta h = -\partial_\theta f $$. Let's also assume that the loss function is $$\theta$$-independent, meaning that $$ \theta $$ parameters only appear if they come from $$ x(\theta, t) $$. This sets $$\partial_\theta L =0$$. An obvious case where this wouldn't hold is for L2 regularization, i.e. weight decay. Finally, let's also put in some boundary conditions. We will say that the initial condition is $$ x_0 =0 $$, which is equivalent to filling the hidden state with all zero's in an RNN. This means  $$g(x_0, \theta) = x_0 $$ so we can kill off $\partial_\theta g$ as well as find that $$ \mu^T = \lambda^T(t=t_i)$$.

We now get the much simpler equations

$$
\frac{dL'}{d\theta} = -\int_{t_i}^{t_f} \lambda^T \frac{\partial f}{\partial \theta} dt
$$

$$
\frac{d\lambda^T}{d t} = -\lambda^T \frac{\partial f}{\partial x} + \frac{\partial \mathcal{L}}{\partial x}
$$

Finally, to align notation with the [paper](https://arxiv.org/pdf/1806.07366.pdf){:target="\_blank"}, we define

$$
a(t) \equiv \frac{\partial L}{\partial x}
$$

and call this the **adjoint sensitivity vector**. In terms of Langrangian mechanics, $$x$$ is a coordinate, and so this is equivalent to a **generalized force**, which makes sense. It is a constraint force meant to impose that the variable's time-evolution obeys an ODE.

If we also use the same loss function as them, which depends only on the final time, then we have to take $$\mathcal{L}$$ out of the integral. This means that the derivative term $$\partial_x \mathcal{L} $$ vanishes, and we find the simple relation that $$\lambda^T = a^T$$. With these conditions, we recover the equations found in the paper:

$$
\frac{dL}{d\theta} = -\int_{t_i}^{t_f} a^T \frac{\partial f}{\partial \theta} dt
$$

$$
\frac{da}{d t} = -a^T \frac{\partial f}{\partial x}
$$
