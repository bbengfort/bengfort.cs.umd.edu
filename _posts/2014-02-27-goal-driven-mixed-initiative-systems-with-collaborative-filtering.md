---
layout: post
title: "Goal Driven Mixed-Initiative Systems with Collaborative Filtering"
modified: 2014-02-27 10:32:10 -0500
tags: [goal driven autonomy, recommenders, mixed-initiative, collaborative filtering, mission impossible]
image:
  feature: mission-impossible.jpg
comments:
category: thoughts
share: 
---

Mixed-initiative systems combine the efforts of human actors with artificial agents to improve the performance of both participants in some mutual functionality. For example, an intelligent search application can be viewed as a mixed-initiative system where a human uses token strings or a special query language to enact searches that are carried out by an ensemble of statistical models which deliver intelligent results. In return, they benefit from the positive feedback of the user, e.g. which links are clicked, or how many queries it takes to find a result. 

A mixed-initiative system is said to be goal-driven when predefined goals are used to affect the behavior of both the human user and the underlying agent. In our search example, we could talk about an “academic goal” where the user is specifically looking for research materials. In this case, the types of queries made by the user will be influenced by this goal, and the results or domain of the agent should also take this goal into account. Additionally, the agent should seek to recover from failures or anomalies by generating its own goals. By using goals, we can effectively communicate bidirectional intent as well as explain failure in mixed-initiative systems. 

A third, interesting dimension comes into play when there are many human users participating in a mixed-initiative system. Namely, user behavior coupled with their goals creates a latent semantic network which can be accessed to improve mixed-initiative interactions through recommendation algorithms. In the next sections, I will describe the novel application of collaborative filtering algorithms improved with goal-dimensionality to mixed-initiative systems to both allow agents to provide other human help to a human user (goal combination) as well to elicit or change the goals of a human user in real time to improve their efficacy (goal generation). 

## Collaborative Filtering for Goal Combination ##

Consider an opening scene from the 1970s TV version of Mission Impossible: Peter Graves pours over the dossiers of IMF agents as the camera hones in on their skills: weapons-expert, master of disguise, a navy diver. He carefully selects the perfect team for his mission as the burnt remnants of the mission tape smolder on the table. He knows that he will only succeed in his mission if he combines the expertise of his agent in a specific mission-oriented configuration.

So too can our goal driven, mixed-initiative systems assist human users on their efforts, particularly in multi-domain systems. Each human user (or even possibly agent or model) has an expertise that is expressed through the goals of each user. A financial analyst will have goals related to their particular expertise, just like a language expert might also have goals related to a particular region. However, because a human user has many, changing goals- we can build a goal-to-goal similarity matrix using the goal-vector of the user; this matrix is the basis of item-centric collaborative filtering algorithms. 

The goal similarity matrix will produce clusters of related goals. The system can then use the goal cluster to discover other human analysts to recommend to the original user either in a passive context (provide hints based on the behavior of the other similar users) or in an active context- directly connecting the recommended user to the original user. Team selection can also be made on this basis- selection of members from related clusters to improve the overall effectiveness of the human team. Because recommended users will have different goals than the original user, this activity can be seen as goal combination. Either way, the human user will experience highly personalized guidance on part of the system through these recommendations.

## Collaborative Filtering for Goal Generation ##

As mentioned in the previous section, a human analyst (or agent’s) goals change over time- either as (1) the result of a new mission, (2) a change in world knowledge or the environment, or (3) in order to explain or recover from failure. It would be useful for a mixed-initiative system to anticipate these changes in goals particularly in order to adapt to (2) or (3). This will lead to a much smoother transition between goals, as well as improve the efficacy of a system that has to respond to a dynamic environment.
 
Changing the goals of the analyst in real-time is possible through the novel use of our goal-based collaborative filtering with an additional dimension- the changing state of the behavior of the user. Our similarity matrix is now not only goal-to-goal, but instead goal-to-goal-to-state. This similarity matrix can then be decomposed to a series of class based goal-to-state similarity matrices. As the state of the user changes (e.g. the nature or classification of the behavior in terms of nearest-similar goals) these more similar goals are presented to the user in order to influence the user’s behavior towards more fruitful behavior (or to allow the user to avoid potential pitfalls when they recognize an ineffective goal).

In this manner, the system can affect the behavior of the user before the user is aware of the need for a change in goals. This results in a highly personalized system that is bidirectional. Not only can the system use the behavior state to recommend goals and influence human behavior, the system can also use the goals to modify the results based on positive feedback from other users with similar goals and similar behavior. For instance, the selection of a recommended goal is a positive feedback mechanism that can be used in active learning when computing goal-state similarity matrices. 
