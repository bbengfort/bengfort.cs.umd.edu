---
layout: post
title:  "Actors for Distributed Dataflow"
tags: [news, research, actors, simulation, distributed systems]
category: research
image:
  feature: actors-dataflow.png
  credit: Simulated communications patterns of actors for our three models.
comments:
share:
paper: actors-dataflow.pdf
---

Recently my research crew (since we're all in different research groups) has become interested in the possibility of reimagining _actors_ for distributed computation. Actor based computation is actually pretty old-school, but it's regaining popularity, especially with distributed computation becoming vital for big data processing. After Kostas' advisor, Dr. Amol Deshpande introduced us to the _Orleans_ paper that uses virtual actors; we decided to investigate further. Our project involved creating a simulation using `Simpy` to study actor patterns. The paper below discusses our initial findings.

You can find the Github Repository with the code for our simulation here: [http://bit.ly/actors-simulation](http://bit.ly/actors-simulation).

## Abstract

Recently, the Actor model of concurrency in distributed systems has regained popularity as cluster computing frameworks for large scale analytics have started becoming mainstream. In particular, the use of virtual actors has shown to provide automatic scaling and load balancing through virtual actor properties of perpetual existence, automatic instantiation, and locale transparency. This programming model appears to be ideal for data processing of streams of unbounded data sets, in a computing architecture of live, online processing of data. In this paper, we present the communication patterns of three such data processing applications using sets (or &ldquo;casts&rdquo;) of Actors. We then model the communication behavior on a cluster using a simulation and show that the virtual actor model effectively encapsulates how a cluster should behave in response to variable volumes of data. We propose that this simulation strongly motivates future work towards generalizing virtual actor spaces for distributed computation.

## Introduction

Sensors, web logs, and other timely data sources are increasingly being used for  real-time or on-demand applications that utilize machine learning models to quickly make predictions and adapt to changes reflected by the input data. Timely sources present a challenge as they are streams of unbounded, occasionally unordered data sets that must be computed upon in an online, real-time fashion rather than in batch. When these data sources are also variable (e.g. the volume of traffic can occasionally spike) then the underlying computing framework must be able to adapt and balance the load or risk falling so far behind that the computation becomes meaningless. Scalability must be _effortless_; it should &ldquo;just work&rdquo; by simply scaling out the cluster size, and be _adaptive_ so that the cluster can be fairly utilized by all available jobs.

Because of the widespread adoption of distributed computing frameworks like MapReduce [1] and Spark [2], distributed computing in a cluster of (often virtualized) economic servers has become the default, general method of high performance data processing. These frameworks primarily abstract the details of parallelism and distributing computation away from the developer, and have allowed a wider community of programmers and scientists to take advantage of parallel algorithms and libraries. Machine learning in particular has come of age in this computing environment, and as a result there exist many high level libraries for fitting a wide array of models in parallel.

As a result, tools for dealing with _streaming data_ (unbounded, unordered, variable data sources) have mostly been contextualized similarly to parallel machine learning algorithms &mdash; via descriptions of how data flows through the application. Tools like Spark Streaming [3], Storm [4], and Google DataFlow [5] implement a _data flow_ programming model, where analysis is described as a directed graph whose nodes represent a single computation, and whose edges represent the transmission of input and output values. Describing computation this way is simple, adaptable, and when mapped to a distributed topology, scalable. However, this model does not allow nodes to _communicate_, nor does it provide any flexibility in the computational topology for error handling, transactional guarantees, consistency, or persistence.

In response to this inflexibility of the data flow model, the _actor model_ [6],[7] has been re-emerging as an abstraction that allows for more versatility in communication between distributed processes while still being at a high enough level to abstract the details of communication within the cluster away from the programmer. Ironically, it is this model that underpins the more general data flow models [8], perhaps due to the fact that these applications are implemented in the Scala programming language, which by default uses actors for concurrency [9], [10]. Actors in this context are high level primitives that do not share state, making them ideal candidates for under-the-hood parallelism. However, a simple data structure alone is not enough for distributed computing; there must also be an execution and job management context. Orleans [11] presents the idea of a &ldquo;virtual actor space&rdquo;, analogous to virtual memory, such that actor activations (e.g. instances of a program) exist forever, are transparent to their locale, and are automatically instantiated and deactivated. Virtual actors therefore provide for automatic scaling and load balancing in the cluster, and we believe are a novel and suitable alternative to the data flow model in the context of online, streaming computation.

In this paper we investigate the potential of virtual actors for distributed computing on streaming data by simulating a computing cluster which implements a _generalized virtual actor space_. A generalized virtual actor space replaces the programing model of virtual actors with a daemon service that runs in the background of all nodes in the cluster; allocating resources to a variety of actor programs in an on-demand fashion such that the resources can scale as variability of data streams changes, while balancing load across many &ldquo;always on&rdquo; computations that must share resources. We have simulated this framework by analyzing the communication patterns of three real-time applications: a traditional data flow, an online recommendation application, and a solar weather forecasting application. We then present an analysis of how the virtual actor model behaved in simulation on the cluster and show that this model is a suitable substitute for the data flow model.

The rest of this paper is organized as follows. In the first section we present the details of our distributed computing model using actors, and describe the generalized virtual actor space in detail. Following this, we describe the applications we analyzed, and how we derived a communication model from each type of application. Finally, we describe our simulation methodology, present our results, and conclude with a discussion and future work.

&hellip;

![Simulated communication patterns of actors for our three models.]({{ site.url }}/images/actors-dataflow.png)

<p style="text-align:center; padding-bottom:18px;">
    <strong><small>Actual communications patterns of actors for our three communication models: dataflow (blue), NNMF (green), and machine learning (red). The color identifies the actor type, and the size the number of messages received. This communication graph was constructed on a simulated cluster of 64 nodes with 4 processes per node.</small></strong>
</p>

&hellip;

**Read the full paper**: [Actors for Distributed Dataflow]({{ site.url }}/papers/{{ page.paper }})

**Github Repository**: [GVAS Actors Simulation](http://bit.ly/actors-simulation)
