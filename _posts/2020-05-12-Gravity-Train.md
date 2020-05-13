---
layout: post
title: "Gravity Train"
date: 2020-05-12
---
<!--# Gravity Train-->


 

## Gravity as transportation agent
Wouldn't it be great to travel from one part of the world to other in just less than an hour? How cool would that be? Well, physics tells us that in a perfectly ideal world it is possible to travel from one end to the other part of the world less than an hour.  

Set apart the practicality the idea is not only simple enough to grasp but has also an easily derivable physical equations  behind it.  In this 'project' gravity is the sole source of energy and the lead in the transportation industry. What is proposed in a nutshell is that: to travel from one part of the world to the other all we need to do is dig a tunnel that connects these two end points. Simple, huh! But that won't be as easy as it sounds, for starter let us skip few steps ahead and think we have the capability to dig through the interior of the Earth which in itself is challenging. But lets us, for the sake of it, assume we are able to surpass this hurdle, next comes the question of throwing out all that is to come from the digging, you see to connect all cities through out the world we are going to dig holes here and there, and can you imagine the soil/dust coming out of all these tunnels? where will we put it? 

Even if we manage to come up with a viable solution, won't it in itself lead us to another problem, won't the gravitational force of the Earth (now a hollow Earth) be affected? If so, wouldn't the project fail? Honestly the question of practicality is going to pile up one after the other , one can raise questions like: would't the temperature in the interior part of the Earth melt away the train itself, how would we deal with the effect of friction?

To proceed let us imagine that we have such advanced technological capability and managed to  tackle every challenge facing us; after all this,  at the end, we will be faced with geographical reality. In a real world situation about 71% of the Earth's surface is covered by water. According to [wikipedia](https://en.wikipedia.org/wiki/Antipodes), of the remaining land part roughly about $15\%$ of it  will have a land on the opposite side of the tunnel. This means even if you dug up a whole from your city chances are high that you will end up on ocean than land, i.e $$0.15 \times 29\% = 4.35\% $$, where $$29\%$$ is the percentage of land on Earth's surface. 

Bear in mind connecting two parts of the Earth with a tunnel that passes through center of the Earth is requires the length of the tunnel to be the diameter of the Earth. And two diametrically opposite points in a sphere (or Earth in this case) what is known as \textbf{Antipodes} in geography and mathematics.  The map on Fig.~\ref{fig:Antipodes_LAEA} shows the antipode of each point on Earth's surface. Land antipodes are depicted as  points where the blue and yellow overlap. The yellow areas are the reflections through Earth's center of land masses of the opposite Western Hemisphere. I refer the interested reader to [Antipodes of each point on Earth's surface](https://commons.wikimedia.org/w/index.php?curid=4343579.)  for further reading.

<!-- ![Antipodes of each point on Earth's surface](/img/Gravity_Train/Antipodes_LAEA.png {:height="36px" width="36px"})-->
<img align="center" src="/img/Gravity_Train/Antipodes_LAEA.png" alt="Antipodes of each point on Earth's surface" width="500"/>.   
*Antipodes of each point on Earth's surface*

In case you are not into doing  hand calculation to get the antipodal of your city, or country, here is a python library that you can make use of to calculate the latitude and longitude of your antipodes. Just in case your antipodes are land,  congratulations go look for someone in the other part of the world and make **Earth sandwich**


``` python
from pygeodesy import antipode
your_antipode = antipode(39.0032, 76.9719)#
print (your_antipode.lat) # -39.0032
print (your_antipode.lon) # -103.0281
```

## The physics of Gravity Train
So assuming we will overcome the technological challenges, let us now employ the physics. In this section we will calculate how long it takes for the gravity train to commute from one end to another via the center of the Earth. For simplicity and convenience we take the Earth's density to be uniform on average.  At the starting end when you enter the tunnel we can calculate your acceleration due to gravity using Newton's universal gravitation law as follows (recall you will be 'falling' into the tunnel)

![A tunnel through the Earth ](/img/Gravity_Train/Earth.png)
*A tunnel through the Earth*
\begin{equation}
\begin{aligned}
 -\frac{GM_em}{R_e^2} &= -mg\\
 g &= \frac{GM_e}{R_e^2}
\label{eq:one}.
\end{aligned}
\end{equation}
 Plugging the values of mass of the Earth $ M_e = 5.967\times10^{24} \mathrm{kg}$,  radius of the Earth $R_e=6,371 \mathrm{km}$, and the gravitational constant $$G=6.674\times10^{-11} \frac{\mathrm{m}^3}{ \mathrm{kg s}^2}$$ we get that your starting acceleration is $9.8 \mathrm{m/s}^2$. But will this acceleration be the same all the way into the center of the Earth? We will answer that in a moment. One thing we should note is that the mass of the Earth changes as we go deep, this is because the distance from the center is decreasing. When you arrive, say at a distance $h$, from the surface. It is implied that you are $$r= R_e-h$$ away from the center of the Earth. Therefore the mass of this 'new' sphere, shown as  white in  Fig.~\ref{fig:Earth} depends on the volume and density of the portion of the Earth, i.e $$m_r = \rho v_r$$, where $$v_r$$ is the volume of the smaller sphere. Following same steps as we did to arrive at Eq.~(\ref{eq:one}), and recalling the assumption that the density of the Earth to be uniform, the gravity at height $$r$$ from the center takes the form: 
\begin{equation}
\begin{aligned}
 g_r = \frac{Gm_r}{r^2},\\  
 g_r =g \frac{r}{R_e}
\label{eq:two}.
\end{aligned}
\end{equation}
where we used 
\begin{equation}
\begin{aligned}
 m_r = \rho\frac{4\pi r^3}{3}= \frac{3M_e}{4\pi R_e^3}\frac{4\pi r^3}{3}\\
\label{eq:three}.
\end{aligned}
\end{equation}
This simple result  entails with it an important  interesting information, it. reveals that as you go to the center of the earth the acceleration becomes smaller and smaller. Using the acceleration of Eq.~(\ref{eq:two}), the gravitational force at distance $r$ can also be calculated as 
\begin{equation}
\begin{aligned}
 F = -mg_r\\  
 F =- \left(\frac{mg}{R_e}\right)r
\label{eq:four}.
\end{aligned}
\end{equation}
You already might have noticed that Eq.~(\ref{eq:four}) has the same form as Hooke's law, with $$k=mg/R_e$$. According to Hooke's law the force required to either extend or compress a spring by some distance $$r$$ scales linearly with respect to that distance, $$F=-kr$$. This implies that once you set foot on that train at the entry, you will oscillate back and forth through the center of the Earth, that means unless you grab onto something upon arrival at your destination you will be sent back again and so on forth. 

This realization, about oscillation, lends itself to help us calculate the time it takes for a round trip, because it means we can rewrite our equation as
\begin{equation}
\begin{aligned}
\left(\frac{mg}{R_e}\right)r &= -m\omega^2 r\\  
\omega =\sqrt{ \frac{g}{R_e}}
\label{eq:five}.
\end{aligned}
\end{equation}

Knowing the angular frequency,  we can calculate the period of your oscillation -that is the time it takes you to make round trip

\begin{equation}
\begin{aligned}
 T =2\pi\sqrt{\frac{m}{k}}\\  
 T =2\pi\sqrt{ \frac{R_e}{g}}\\  
 \approx  5068 ~\mathrm{sec} \approx  84.5 ~\mathrm{min}  
\label{eq:six}.
\end{aligned}
\end{equation}

This means the time taken for you travel to your destination is half of the round trip or about $42$ minutes, pretty nice huh?! Hang on though, you will feel  weightlessness when you are passing through the center.

## Orbiting around the Earth
Another proposal which can also be found on [hyperphysics](http://hyperphysics.phy-astr.gsu.edu/hbase/Mechanics/earthole.html#c1) is instead of drilling a hole, you can go around the orbit, close to the surface of the Earth, and then drop when you arrive at your destination.  Once again for simplicity we  consider an ideal situation where we stop worrying about air drag and stuff like that. 

Of course we can approach the problem from circular motion perspective, i.e  $F_g=\frac{mv^2}{R_e^2}$.  But for this  section let us use the conservation of mechanical energy, just to use variety of approaches. To reiterate we are ignoring all challenges like temperature, pressure etc. As this assumption allows us to  employ the total mechanical energy conservation, let us exploit that approach in here. In a nutshell, for an isolated system, the total mechanical energy conservation dictates that  $$\Delta K + \Delta U =0$$.  

To implement the conservation of energy, we are going to use the following parameters. At the entry point let us say your speed is zero and your height is the diameter $2R_e$, at the exit hole your height is zero and speed is to be determined. I say entry and exit, because we are assuming that you are going to start with zero speed at your starting point, which is  a diameter away from your destination  

\begin{equation}
\begin{aligned}
  (\frac{1}{2}mv_f^2 -  \frac{1}{2}mv_i^2) + (mgh_f-mgh_i)=0\\ 
  2mgR_e = \frac{1}{2}mv^2 \\  
  v = \sqrt{gR_e}
\label{eq:seven}.
\end{aligned}
\end{equation}
From which we now make use of one of the kinematic equations for uniformly accelerated rectilinear motions to determine the time of your travel. It is worth noting here too that the distance you travel to arrive at your destination is the circumference of the circumference of the circular path, i.e $$2\pi R_e$$.
\begin{equation}
\begin{aligned}
  T =\frac{2\pi R_e}{v}\\  
  T = 2\pi\sqrt{\frac{R_e}{g}}\\  
\label{eq:eight}.
\end{aligned}
\end{equation}
This exactly same as the period of your back and forth trip.





<script src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

