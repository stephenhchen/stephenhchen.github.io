---
title: "Deriving Gradient Descent"
date: 2022-08-31T23:29:33-06:00
slug: ""
description: ""
keywords: []
draft: false
tags: []
math: true
toc: false
---

### Stepping through gradient descent

The goal for this post is to describe ideas and derive gradient descent. By the end, we’ll have built a foundation for understanding the intuition for understanding other algorithms like Newton’s method and mirror descent. 

When optimizing a function, knowing more about \\(f\\) can make optimization much easier. Imagine if we start with knowing nothing about \\(f\\). That is, we let \\(f:\mathbb{R^n}\rightarrow \mathbb{R}\\) be all we have. We won’t have a lot to work with since that means we’re permitting \\(f\\) to be as complicated as it can be. So a starting point is to restrict \\(f\\) to be nicely differentiable (to be in the class of differentiable functions from \\(\mathbb{R^n}\rightarrow \mathbb{R}\\)).

Now that we have \\(\nabla f\\), a reasonable first-stab can be something like this: \\(f\\) is quite complex so instead let’s try to minimize a linear approximation of \\(f\\) and see where that leads us.

The first-order Taylor approximation \\(T_{1}(x;x_0)\\) of \\(f\\) around some point of interest \\(x_0\\) is 

\\(
	T_1(x;x_0) =f(x) + \nabla f(x_0)^T (x-x_0)
\\)

We could try to minimize this instead. But this leads us nowhere since minimizing a over an unbounded subspace will just give us an unbounded solution. Also we know that \\(T_1\\) is only representative of \\(f\\) when we’re quite close to \\(x_0\\).

To fix this, let’s penalize how far we are from the point of interest \\(x_0\\) so that we don’t stray too far from \\(x_0\\)  during the minimization of \\(T_1\\). Let \\(g(x;x_0)\\) be the new function:

\\(
g(x) = f(x) + \nabla f(x_0)^T(x-x_0) + \frac{1}{2\eta}||x-x_0||^2
\\)

So we’ve expressed \\(f\\) as a first-order Taylor expansion and a proximity term with a new variable \\(\eta\\) which we decide on how much to penalize moving away from \\(x_0\\). 

What we’re essentially doing is equally penalizing all directions by using the Euclidean norm here, but we could generalize this idea with any norm, each of which changing the geometry of how gradient descent views area of loss. 

The idea of rescaling space locally can greatly help gradient descent move faster towards an optimal solution. This is important in building towards Newton’s method and other algorithms such as proximal gradient descent and mirror descent. 

Now this quadratic function has a unique minimum, which we can solve for by finding \\(x\\) where the gradient is not changing. 

\\(
\begin{aligned}
x^* &= \argmin g(x) \\\
&=\argmin_x f(x) + \nabla f(x_0)^T(x-x_0) + \frac{1}{2\eta}||x-x_0||^2 \\\
&\implies 0 = \nabla g(x) = \nabla f(x_0) + \frac{1}{\eta}x \\\
\end{aligned}
\\)

Solving the last equality for \\(x\\) we obtain our solution \\(x^*\\) to this minimization problem. 

This gives us \\(x^* := -\eta\nabla f(x_0)\\). 

So if we’re approximating \\(f\\) at \\(x_0\\) and we want to minimize \\(g\\), **which we defined in terms of \\(x_0\\),** the solution should be the point which is precisely the negative gradient direction, which we know points in the direction of steepest descent.

This solution is relative to the given starting point \\(x_0\\). So this naturally suggests an iterative procedure for minimization. So far we’ve been purely in the math world, but we can now write down a straightforward algorithm given everything learned.

\\(
\begin{aligned}
x_{t+1} &=\argmin_x f(x) + \nabla f(x_0)^T(x-x_0) + \frac{1}{2\eta}||x-x_0||^2 \\\
x_{t+1} &= x_t -\eta\nabla f(x_t)
\end{aligned}
\\)

We probably didn’t need all of this just to introduce gradient descent. But it’s nice to see that we can build this up carefully using only simple tools from calculus. Really, the whole reason for doing all this is to introduce the proximal term, or the idea of changing space locally to decide the next step. We’ll cover that in the next post.

### Summary

Gradient descent amounts to the simplest greedy algorithm in continuous optimization. We look at the line from where we are that points in the best descending direction, and just take that step while not straying too far from where we are. Crucially, how we defined “straying too far” depended on the choice of norm. If we changed norm, or chose another way change nearby space around where we are, we may choose other candidate points, which may in fact be better!