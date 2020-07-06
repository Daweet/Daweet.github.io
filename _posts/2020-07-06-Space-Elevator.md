---
layout: post
title: "Space Elevator"
category: Data Science, Physics
tags: [gravity, space elevator, Kepler's law, space tower]
date: 2020-07-06
---

Leaving Earth’s gravitational pull requires tremendous energy not to
mention the cost associated with it. At present we use rockets to propel
object into space. But there is an alternative floating around–space
elevator or space tower. The proposal is to build a very tall rising
tower from Earth’s equator terminating geostationary orbit.

The idea of the space elevator was first proposed by the Russian
physicist Konstantin Tsiolkovsky and again by the Leningrad engineer,
Yuri Artsutanov [To the cosmos by electric
train](https://web.archive.org/web/20060506100948/http://liftport.com/files/Artsutanov_Pravda_SE.pdf).
It was then popularized by Arthur Clarke’s novel *Fountains of
Paradise*. Yet until the discovery of carbon nanotubes the realization
of space tower was considered as farfetched idea. [Carbon
nanotubes](https://en.wikipedia.org/wiki/Carbon_nanotube) (CNTs) are
tubes made of carbon with diameters typically measured in nanometers.
Carbon nanotube is the best candidate so far, that is being proposed as
a construction material; this is so because it has high tensile strength
along with low density.

![[fig:Spacetower] [A space elevator is conceived as a cable fixed to
the equator and reaching into
space.](https://en.wikipedia.org/wiki/Space_elevator.)](/img/space_elevator/)

How to construct the elevator? the proposal is to build a free standing
tower of uniform density and constant cross sectional area at Earth’s
equator. But, what is free standing tower? a [free standing
tower](https://pdfs.semanticscholar.org/d402/ba5f97884b7398ae2a1ff79136f9c1a03993.pdf)
is one whose weight is counter-balanced by the outward centrifugal force
on it. This means the tower won’t exert force on the ground beneath it.
Such type of tower is in tension along its entire length. Each element
of the tower is in equilibrium under the action of gravitational,
centrifugal, and tension forces.

The forces acting on a small element of the tower are: an upward force
$$\mathrm{F}_U$$ due to the portion of tower above the element, a downward
force $$\mathrm{F}_D$$ due to the portion of tower below, a downward force
$\mathrm{W}$ due to the weight of the element, and a fictitious upward
centrifugal force $$\mathrm{F}_C$$ on the element due to its presence on
the rotating Earth. The vector sum of these forces must vanish if the
element is in equilibrium.

The weight and the centrifugal forces balances each other at the
geostationary height i.e. $$\mathrm{F}_C = \mathrm{W}$$ . This implies the
tension forces at the two ends should also be equal, i.e.
$$\mathrm{F}_U = \mathrm{F}_D$$. For an element below the weight force
exceeds the centrifugal force, thereby $$\mathrm{F}_U > \mathrm{F}_D$$ for
an equilibrium. In contrary for an element above the geostationary
height, the centrifugal force exceeds the weight and thus
$$\mathrm{F}_U < \mathrm{F}_D$$ for an equilibrium.

The physics of space elevator
-----------------------------------------

Let us denote mass, radius and rotational angular velocity of the Earth
by $$\mathrm{M}, \mathrm{R}$$ and $$\omega$$

$$\begin{aligned}
 -\frac{GMm}{R_g^2} &= -m\omega^2R_g\\
 R_g\ &= \Big(\frac{GM}{\omega^2}\Big)^\frac{1}{3}
\label{eq:one}.
\end{aligned}$$

Where $$\mathrm{G}$$ is the gravitational constant
$$G=6.674\times10^{-11} \frac{\mathrm{m}^3}{ \mathrm{kg s}^2}$$. Let us
assume that the standing tower has constant mass density $\rho$ and
cross sectional area $$\mathrm{A}$$. Suppose a small element of tower of
length $$dr$$ whose lower end is a distance $$r$$ from the center of the
Earth.. Equilibrium condition demands that:
$$\mathrm{F}_U - \mathrm{F}_D +\mathrm{F}_C - \mathrm{W} =0$$. Moreover if
the tensile stress $\mathrm{T}$ is defined as force per unit area we
write $$\mathrm{F}_U - \mathrm{F}_D = AdT$$. The equilibrium condition can
be put as

$$\begin{aligned}
 AdT &= \frac{GM(Adr\rho)}{r^2}-(Adr\rho)\omega^2r, \\
 \frac{dT}{dr}&=GM\rho\Big[\frac{1}{r^2}-\frac{r}{R_g^3}\Big] 
\label{eq:two}.
\end{aligned}$$

upon integrating Eq. ([eq:two]) from $$r=R\rightarrow R_g$$, with
$$\mathrm{T(R)}=0$$, we get the tensile stress at geostationary height
$$R_g$$

$$\begin{aligned}
 \mathrm{T(R_g)} =GM\rho\Big[\frac{1}{R}-\frac{3}{2R_g}+\frac{R^2}{2R_g^3}\Big] 
\label{eq:three}.
\end{aligned}$$

Let $$\mathrm{H}$$ denote the distance of the top of the tower from
Earth’s center. We can determine $$\mathrm{H}$$ by integrating
Eq. ([eq:three]) from $$r=R_g\rightarrow \mathrm{H}$$ subject to the
boundary condition $$\mathrm{T(H)}=0$$, which expresses the fact the
tension drops to zero at the upper end of the tower. In this way we find

$$\begin{aligned}
\mathrm{T(R_g)} =GM\rho\Big[\frac{1}{H}-\frac{3}{2R_g}+\frac{H^2}{2R_g^3}\Big] 
\label{eq:four}.
\end{aligned}$$

If we equate the RHS of Eq. ([eq:three]) and Eq. ([eq:four]) and note
that $$\mathrm{H}=\mathrm{R}$$ is a solution of the resulting in cubic
$\mathrm{H}$, we can reduce the cubic to the quadratic equation

$$\begin{aligned}
 RH^2+R^2H-2R_g^3 &= 0\\
 H&=\frac{R}{2}\Big[\sqrt{1+8\Big(\frac{R_g}{R}\Big)^3}-1\Big]=150,000~\mathrm{km}
 \label{eq:five}.
\end{aligned}$$

the height of the top of tower above Earth’s surface is therefore
$$\mathrm{H}-\mathrm{R}=140,000~\mathrm{km}$$

Center of Mass of the Tower?
----------------------------------------

Another point worth looking at is the question of center of mass of the
tower based on [Kepler’s law of
motion](https://gassend.net/spaceelevator/center-of-mass/index.html).
Kepler’s law is valid for objects whose masses are small in comparison
to the variations of the gravity field the objects are moving in. In the
case of the space tower, gravity decreases by a factor greater than 300.
Also recall that the space elevator is not a point mass.

Take for example two identical masses attached by a length $$2L$$ of
massless cable. Assume one of the masses is at a distance $$R+L$$ from the
center of the Earth and the other at $$R-L$$. If the the masses are in
circular orbit of angular velocity $\omega$ in such a way that the cable
always goes through the center of the Earth, the centrifugal
acceleration of the masses is

$$\begin{aligned}
  a_{cL} &=(R-L)\omega^2\\
 a_{cH} &=(R+L)\omega^2\\
\label{eq:six}.
\end{aligned}$$

Where $$L$$ and $$H$$ in subscripts indicate Low and High. In like manner the
acceleration due to gravity on the masses is expressebile as

$$\begin{aligned}
  g_{cL} &=\frac{GM_e}{(R-L)^2}\\
 g_{cH} &=\frac{GM_e}{(R+L)^2}\\
\label{eq:seven}.
\end{aligned}$$

At equilibrium we demand that these accelerations cancel each other out

$$\begin{aligned}
 (R-L)\omega^2 + (R+L)\omega^2 &= \frac{GM_e}{(R-L)} + \frac{GM_e}{(R+L)}\\
 \omega^2 &= \frac{GM_e}{2R}\Big(\frac{1}{(R-L)^2}+\frac{1}{(R+L)^2}\Big)
 \label{eq:eight}.
\end{aligned}$$

For $$L=0$$, we obtain $$\omega^2 =\frac{GM_e}{R^3}$$, which is Kepler’s
third law. If L is small then

$$\begin{aligned}
  \frac{1}{(R+L)^2}\equiv\frac{1}{R^2}-\frac{2L}{R^3}
\label{eq:nine}.
\end{aligned}$$

Which also is Kepler’s third law. For large values of $$L$$ we note that
$$\omega$$ is greater than Kepler’s third law would predict. The reason
for this is because the increase in gravity for the low mass is greater
than the decrease for the high mass.

In conclusion for an orbit that extends over a great range of altitudes,
the orbit will be faster than the orbit at the center of mass altitude.
Thus, even for an elevator not attached to the Earth, the center of mass
should be above geosynchronous orbit (GEO).

<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
