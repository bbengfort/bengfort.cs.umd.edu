---
layout: post
title:  "Evolutionary Design of Self-Organizing Particle Systems for Collective Problem Solving"
tags: [news, research, evolutionary algorithms, particle swarms]
category: research
image:
comments:
share:
paper: evolutionary-design-swarms.pdf
---

<iframe src="//www.slideshare.net/slideshow/embed_code/key/M99oimH59QL5V1" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:15px"> <strong> <a href="//www.slideshare.net/BenjaminBengfort/evolutionary-design-of-swarms-ssci-2014" title="Evolutionary Design of Swarms (SSCI 2014)" target="_blank">Evolutionary Design of Swarms (SSCI 2014)</a> </strong> from <strong><a href="//www.slideshare.net/BenjaminBengfort" target="_blank">Benjamin Bengfort</a></strong> </div>

In December, I attended IEEE SSCI 2014 - a symposium on computational intelligence where I presented work that my classmates and I put together for Jim Reggia's class: _Artificial Life and Evolutionary Computation_. The goal of the paper was to show off the use of evolutionary and genetic algorithms for _creativity_, that is the use of a computer program to design a system without very much human input. I've attached the abstract, paper, and presentation from that conference here. However, the purpose of this post isn't to point out the scientific contributions - but rather the practical ones.

In order to do this project, we had to create a pretty extensive Python software base with the following components:

1. A simulator that could both visually and computationally determine the fitness of a particle swarm
2. A multiprocess evolutionary mechanism for designing the particle swarms

In the end our deployment used PyGame, Celery, and an Amazon EC2 2XL server which ran for a week to finish the computation. The paper doesn't necessarily touch too much on the development effort required. Given the opportunity to do this again, I probably would have used Go or Cython to improve the performance (not much you can do in a single semester). This post collects both the Github repository and development environment with the scientific details. Hopefully it's a useful reference! 

## Abstract

Using only simple rules for local interactions, groups
of agents can form self-organizing super-organisms or flocks
that show global emergent behavior. When agents are also
extended with memory and goals the resulting flock not only
demonstrates emergent behavior, but also collective intelligence:
the ability for the group to solve problems that might be
beyond the ability of the individual alone. Until now, research
has focused on the improvement of particle design for global
behavior; however, techniques for human-designed particles are
task-specific. In this paper we will demonstrate that evolutionary
computing techniques can be applied to design particles, not only
to optimize the parameters for movement but also the structure of
controlling finite state machines that enable collective intelligence.
The evolved design not only exhibits emergent, self-organizing
behavior but also significantly outperforms a human design in
a specific problem domain. The strategy of the evolved design
may be very different from what is intuitive to humans and
perhaps reflects more accurately how nature designs systems for
problem solving. Furthermore, evolutionary design of particles
for collective intelligence is more flexible and able to target a
wider array of problems either individually or as a whole.



### Project

The full project is available on Github if you'd like to try it yourself. The Github repository contains instructions for how to install the dependencies, and how to run the simulator as well as the evolver. The dependencies might be the main issue, installing PyGame for the visual simulation can be tricky, and Celery is needed for the multi-process evolver. We will respond to any issues that you add as soon as we can!

<style>

    img {
        margin-top: 0px;
        margin-bottom: 20px;
    }

</style>

[![GitHub issues](https://img.shields.io/github/issues/mclumd/swarm-simulator.svg)](https://github.com/mclumd/swarm-simulator/issues)
[![GitHub stars](https://img.shields.io/github/stars/mclumd/swarm-simulator.svg)](https://github.com/mclumd/swarm-simulator/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/mclumd/swarm-simulator.svg)](https://github.com/mclumd/swarm-simulator/network)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/mclumd/swarm-simulator/master/LICENSE)

The code itself is provided under the MIT license. The various badges describing the Github repo are above.

&hellip;

[**Try the project yourself on Github**](https://github.com/mclumd/swarm-simulator)

[**Read the full paper on IEEE Xplore**](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?arnumber=7011790)

[**View the presentation on Slideshare**](http://www.slideshare.net/BenjaminBengfort/evolutionary-design-of-swarms-ssci-2014)
