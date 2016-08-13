---
layout: post
title: 利用redcarpet渲染markdown
date: 2016-01-20 15:32:24.000000000 +09:00
use_math: true
---

Example:
---

When $a \ne 0$, there are two solutions to \(ax^2 + bx + c = 0\) and they are
$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$

this is $latex$

this is $$x= c^2$$

$x=x+c^2+frac(t)$

$
d=mc^2
$

$$
d=mc^2
$$

this is $x= {-b \pm \sqrt{b^2-4ac} \over 2a}.$

$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$

$$
   |\psi_1\rangle = a|0\rangle + b|1\rangle
$$

Let's test some inline math $x$, $y$, $x_1$, $y_1$.

Now a inline math with special character: $|\psi$, $x'$, $x^\*$.

Test a display math:
$$
   |\psi_1\rangle = a|0\rangle + b|1\rangle
$$
Is it O.K.?

Test a display math with equation number:
\begin{equation}
   |\psi_1\rangle = a|0\rangle + b|1\rangle
\end{equation}
Is it O.K.?

Test a display math with equation number:
$$
  \begin{align}
    |\psi_1\rangle &= a|0\rangle + b|1\rangle \\\\
    |\psi_2\rangle &= c|0\rangle + d|1\rangle
  \end{align}
$$
Is it O.K.?

And test a display math without equaltion number:
$$
  \begin{align\*}
    |\psi_1\rangle &= a|0\rangle + b|1\rangle \\\\
    |\psi_2\rangle &= c|0\rangle + d|1\rangle
  \end{align\*}
$$
Is it O.K.?

Test a display math with equation number:
\begin{align}
    |\psi_1\rangle &= a|0\rangle + b|1\rangle \\\\
    |\psi_2\rangle &= c|0\rangle + d|1\rangle
\end{align}
Is it O.K.?

And test a display math without equaltion number:
\begin{align\*}
    |\psi_1\rangle &= a|0\rangle + b|1\rangle \\\\
    |\psi_2\rangle &= c|0\rangle + d|1\rangle
\end{align\*}
Is it O.K.?

[参考资料][ref]

[ref]: http://haixing-hu.github.io/programming/2013/09/20/how-to-use-mathjax-in-jekyll-generated-github-pages/
