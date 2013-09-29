---
title: Numerical Differentiation
description: Test mathjax plugin
date: '2013-09-29'
---

## Derivative of a real function

The derivative of `$f(x)$` is

````mathjax
\begin{equation}
f'(x) = \lim_{h \rightarrow 0} \frac{f(x + h) - f(x)}{h} \label{eq:DerivativeDef}
\end{equation}
````
suggest a naive approach to compute a numerical derivative: pick a small value `$h$` and apply \eqref{eq:DerivativeDef}. However, applied uncritically, this procedure is almost guaranteed to produce inaccurate results.

[Ridders](#Ridders) applied Romberg's method to improve the accuracy in the computation of the first and second derivatives of a real function. Using the Taylor expansion in the vicinity of `$x$`:

$$
f'(x) \approx \frac{f(x + h) - f(x - h)}{2h}
$$

with a truncation error of `$e_t \sim h^2 f^{(3)}$`. As a consequence, `$e_t$` decreases quadratically with decreasing `$h$`.