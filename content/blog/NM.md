---
title: "Newton's Method"
date: 2022-09-14T23:29:33-06:00
slug: ""
description: ""
keywords: []
draft: false
tags: []
math: true
toc: false
---
## Using more of what we know about \\(f\\)

The aim of this post is to gain intuition for understanding Newton’s method.

We look back at how gradient descent was derived previously where we chose to approximate \\(f\\) locally around a given point \\(x_t\\) with a linear function:

\\(
T_1(x;x_t) =f(x_t) + \nabla f(x_t)^T (x-x_t)
\\)

We think of \\(x_t\\) as a current step in an iterative algorithm.

Naturally, \\(x\\) can be chosen to minimize the approximation, but the solution \\(x^*\\) will end up too far from the given \\(x_t\\) since a linear function will be unbounded. To avoid this we added a proximal term \\(\Phi(x,x_t) = \frac{1}{2\eta}||x-x_t||^2\\) to ensure we stay close to \\(x_t\\). We now have:

\\(
g(x,x_t) = f(x_t) + \nabla f(x_t)^T (x-x_t) + \frac{1}{2\eta}||x-x_t||^2
\\)

Now we solve a much simpler optimization problem: \\(min_x g(x,x_t)\\)

Taking the gradient of \\(g\\) and setting it to zero:

\\(
\begin{aligned}
0 = \nabla f(x_t) + \frac{1}{\eta}(x-x_t) \\\
\frac{x}{\eta} = \frac{x_t}{\eta} -\nabla f(x_t) +  \\\
x = x_t -\eta\nabla f(x_t)
\end{aligned}
\\)

But nothing prevented us from minimizing a better approximation to \\(f\\) (around some point \\(x_t\\)), so  long as \\(f\\) is at least twice differentiable. Assuming this is the case, we have:

\\(
T_2(x;x_t) =f(x_t) + \nabla f(x_t)^T (x-x_t) + \frac{1}{2} (x-x_t)^T\nabla^2 f(x_t) (x-x_t)
\\)

Note that this is precisely the same as \\(T_1\\) with the divergence term, where we use **the identity Hessian**. That is, the default to penalize space equally in all directions is the same as having no 2nd order information about \\(f\\).

As before we can minimize \\(T_2\\)  by setting its gradient to zero. The last line gives us the gradient update for Newton’s Method:

\\(
\begin{aligned}
0 = \nabla f(x_t) + \nabla^2 f(x_t)^T(x-x_t) \\\
\nabla^2 f(x_t)x = \nabla^2 f(x_t)x_t -\nabla f(x_t)  \\\
x = x_t -(\nabla^2 f(x_t))^{-1}\nabla f(x_t)
\end{aligned}
\\)

So at every step we need to compute invert the Hessian of \\(f\\), which is quite expensive — \\(O(n^3)\\) 


👉 Note that it’s standard to assume “oracle access” when studying complexity for both the gradient and function values of \\(f\\), meaning we ignore the costs of computing the gradient (which, for modern learning settings with neural networks, is significant!) 
Continuing with this oracle model, we also ignore the costs for computing higher order derivatives as well, which for the Hessian is on the order of \\(O(n^2)\\). Computing the inverse is also awful — \\(O(n^3)\\)

## Some intuition on the update of Newton’s Method:

So at each step of Newton’s method, we take a matrix-vector product using the inverse Hessian. This means we are changing the space around \\(x_t\\) with a linear transformation (a pretty special one actually). Let’s see what exactly this means.


👉 Note: Often we assume \\(f\\) convex, which means \\(	\nabla^2 f(x_t)\\) is positive semi-definite, but this gives no guarantee on the invertibility of \\(H\\) (\\(H\\) needs to be positive definite). A sufficient condition is that \\(f\\) if \\(\alpha\\)-strongly convex, which by definition gives us a *lower* bound on the smallest eigenvalue being positive, but this is not a necessary condition. The fact that we are even considering using Newton’s Method on \\(f\\) assumes that \\(f\\) is nice enough that we can invert \\(H\\) (even if it’s expensive).


Let’s fix some \\(x_t\\) in our trajectory. Since \\(H\\) is symmetric, we can diagonalize it according to to the spectral theorem. So let 

\\(
H = QDQ^T
\\)