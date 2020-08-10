---
layout: post
title: "Galileo's paradox"
category:  Physics
tags: [gravity,  Galileo's paradox, frcition, Energy conservation]
date: 2020-08-09
---

Isolated system
---------------------------------------------------------------

Galileo's paradox, also referred to as Galileo's theorem, asserts that
given a vertical circle the time taken for a body to descend to the
lowest point of the circle along any chord is the same
. The use of Galileo's paradox as experimental tool to
aid lesson is described in [Greenslade](https://aapt.scitation.org/doi/10.1119/1.2909748). The system was also
discussed in detail taking into consideration the effect of the
environment [Aguiar](https://iopscience.iop.org/article/10.1088/0143-0807/35/6/065024). In both papers solution was obtained using
kinematic approach. Yet the experiment can also be used to teach
conservation of mechanical energy. To this end we discuss here the
approach from energy's perspective.To avoid ambiguity with other
paradoxes attributed to Galileo, we follow the suggestion of the authors
in [Aguiar](https://iopscience.iop.org/article/10.1088/0143-0807/35/6/065024) in renaming it as Galileo's kinematical paradox.

To begin with, let us refer to
the figure shown below where we assume the chords to be frictionless, a
particle traveling to the bottom of the circle along the diameter $$D$$,
and another particle traveling along either of the chords will arrive at
the same time if released, simultaneously, from rest . Generally
speaking conservation of total mechanical energy dictates that the sum
overall energy must add up to zero $$\begin{aligned}
\Delta K+\Delta U =0
\label{eq:one}.\end{aligned}$$ Employing
the above equation, we
arrive at $$v_B=\sqrt{2gD}$$. Using which the time taken to travel along
the diameter is shown to be $$t_{AB}=\sqrt{\frac{2D}{g}}$$. Moreover the
time taken for the particle to travel, from $$A$$ to $$D$$ is readily takes
the form $$t_{AD}=\sqrt{\frac{2D}{g}}$$. In a similar manner for the
motion from $D$ to $B$ we note that $$v_B=\sqrt{2gL\sin\theta}$$
consequently
$$t_{DB}={\sqrt{\frac{2L}{g\sin\theta}}}=\sqrt{\frac{2D}{g}}$$.

![[\[fig:circle\]]{#fig:circle label="fig:circle"} Particles, released
from rest simultaneously, from points $$A, C, D, E$$ will take same time
to descend to the lowest point of the circle, i.e $$B$$
.](/img/GalileosParadox/Circle.png)

At this point it is worth recalling that Galileo has also pondered about
the travel from top to bottom of the vertical circle through two chords,
this means a particle following path $$ADB$$ for instance, in such cases
we split the total travel into two stages, i.e $$A\rightarrow D$$ then
$$D\rightarrow B$$. Application of
the first equation in this page, on
both stages separately and collecting what we get yields
$$t_{ADB} = \sqrt{\frac{2D}{g}\left(1+\sin^2\theta\right)}$$. Therefore
even if the distance covered is the same in traveling to the terminal
point, either via the diameter ($$AB$$) or through two chords $$ADB$$, the
time it takes to traverse $$ABD$$ is a bit higher.

System with friction
--------------------------------------------------------------------

In the previous section we have dealt with an isolated system. But if we
are interested in performing the experiment then friction is unavoidable
and must be taken into account. To proceed we should make sure that our
equation for conservation of total mechanical energy reflects this new
information, i.e effect of friction. By this what mean is that, if our
particle slides in a chord with friction, the conservation of total
mechanical energy is modified to take the form
$$\Delta K+\Delta U + \Delta E_{int}=0$$. In doing so the speed at the
bottom of the circle -- for the path across the diameter becomes
$$v_B^2=\sqrt{2gD\left(1-\mu_k\right)}$$. Though the speed, as expected,
is dependent on the coefficient of kinetic friction, what is interesting
however is that the result is independent of $$\theta$$.

To see if we see similar relation we follow the same footsteps and
calculate the time taken to descend across each chords, if for instance
the chord in consideration is from point $$A$$ to $$D$$, we obtain
$$\begin{aligned}
v_D^2 &= 2g\left[\frac{L\cos^2\theta-\mu_kD\sin^2\theta\cos\theta}{\sin\theta}\right]\\
t_{AD} &= \sqrt{\frac{2D/g}{1-\mu_k\tan\theta}}
 \end{aligned}$$

Again for the path along the chord $$D\rightarrow B$$ the time taken to
descend can be shown to be
$$t_{DB} = \sqrt{\frac{2D/g}{1-\mu_k\cot\theta}}$$. This implies that the
inclusion of friction will break the paradox.



<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
