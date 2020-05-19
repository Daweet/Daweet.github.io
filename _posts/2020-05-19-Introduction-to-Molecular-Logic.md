---
layout: post
title: "Introduction to Molecular Logic: --_part I_"
category: physics
tags: [computer, molecule logic, optical logic]
date: 2020-05-19
---


## <span style="color:blue">Introduction</span>

In this post, and sequel of it, I will try to introduce you to **molecular logic** which is one of the candidates that tries to fill the void expected to happen once we reach atomic size miniaturization. 
The sucess of the way towards miniaturization and reduced power consumption of logic
machines is driven by Moore’s law, a prediction made by Intel co-founder Gordon E Moore. This is the observation that the number of transistors on an integrated circuit (IC)
doubles about every [two year](https://en.wikipedia.org/wiki/Moore%27s_law). The processing 
speed of a computer increases as the distance between transistors on IC decreases. Thereby Moore’s law explained how products can simultaneously drop in price while improving the performance. The
correlation provided by Moore’s law is used as a benchmark, by companies that produce
transistors, for the progress of the industry. This trend has so far been very successful 
[see Fig.1](https://humanswlord.files.wordpress.com/2014/01/moores-law-graph-gif.png);
Intel has recently managed to produce a transistor 14nm in size.
<figure>
  <img src="/img/moloclogic/Moore.png" alt="States" style="width:60%">
  <figcaption>Fig.1 -Moore's law.</figcaption>
</figure>
The future of the computing industry depends on whether the growth rate estimated by
Moore’s law could be continued in the future. Based on the trend so far, it is expected
that by 2022, transistors of 5nm in size would be manufactured. And as we go further
down in reducing the size of the transistor, one enters the realm of the size of atoms. At this point the current technolgy and advancement that is mainly based on  miniaturization fails to proceed.
Consequently we need a paradigm shift in the way we manufacture and do computing
once we reach the atomic scale. 

One way, currently being pursued, is to build via the
bottom-up approach, as advocated by Feynman in his famous lecture; 
[_there is plenty of room at the bottom_](https://www.zyvex.com/nanotech/feynman.html). 
In this approach one can manipulate the atoms/molecules individually and build up the desired machine.
The result is ongoing, research all over the world trying to address the problem: Quantum
Computing, Neural Networks based computing, DNA based computing, Quantum Cellular
Automata (QCA), Single Electron Transistor (SET).

In this post I will introduce you to an alternative approach that is set to tackle the aforementioned challenge. This route is usually referred to as molecular logic. But, of course, even within this category there are several branches depending on the input/output.  You see we can interact with molecules chemically (where we use chemical reactions as input/output for our logic), optically (where laser pulses are shone onto the quantum system), and/or electrically (such as electrical voltage in quantum dots). 

<figure>
  <img src="/img/moloclogic/states.png" alt="States" style="width:40%">
  <figcaption>Fig.2 - States we consider.</figcaption>
</figure>

In all these cases, what is being proposed is use quantum systems, their discrete energy levels to be specific, to serve as a switch, logic gates, transistor, and/or integrated circuit that perform logic operations. Optically addressed systems are going to be preferable if the demand is speed, but if market readiness comes into play electrical addressing is the one we are familar with.

Another variation that can be noted on these routes is the physical system in consideration of each branch.
Depending on the experimental setup the quantum system takes either solid or solution state. While it is tempting to favor solids to be advantageous, as they can readily set into production using the current technological practice--thus market ready. Yet it is worth mentioning that the logic operation is going to be wired in different set up than the usual chips. On the contrary, solutions not only require a new technological innovation but also are going to be so messy with several (un)controlling parameters in play. 

For this introduction my focus lies in an optically addressed quantum systems. The physical system we consider is a quantum system and the proposed logic will be a classical input/output. By classical logic we mean the Boolean and Non-Boolean logic operations. This is distinct from Quantum computer as we are not using qubits.

Because of the discreteness of quantum systems, with the appropriate experimental setup, one gets acesses to multistates in nano-devices. This multistate system can be addressed either optically or electrically, to do logics. In a nutshell this can be done by taking advantage of the discreteness of the internal states of a physical system, along with the ability of controlling the form of the perturbation. For example by applying perturbations sequentially scientists have proposed several forms of Finite State Machine: simple Set-Reset latches, Optical Flip Flops, [Full adder and Full subtraction](https://journals.aps.org/pra/abstract/10.1103/PhysRevA.73.033820). Moreover going beyond binary it was demonstrated how one could do multivalued logic using electrical addressing. Ontop of this it has been shown how we can perform a complicated logic operations, like [making decision](https://www.pnas.org/content/110/43/17183/tab-article-info).

The implication in here is that, the more measurable quantities you are able to acess, and control, the more complicated logic operations you are going to design and implement. In optical case this requires an appropriate choice of pulse profile, with which we manage to disturb and control the dynamics of quantum states and, thereby able to perfom logic operations. Having more observables (i.e measurable quantities), consequently, is advantageous if one wants to perform parallel logic. This is so because one can make use of each of the observables to perform logic in parallel. That means each output line performs one logic function.

But just like other kind of routes molecular logic is also still in its infant stage, meaning it is not market ready and will take sometime to reach that level. Apart from this another challenge is how to preserve the stored information? This question is not limited to molocular logic but also is the issue in quantum computer too. The problem is once you store information in a quantum system it will be lost after a while. This happens because of the interaction with the environment. Protecting the stored information is shown to be challenging.  


