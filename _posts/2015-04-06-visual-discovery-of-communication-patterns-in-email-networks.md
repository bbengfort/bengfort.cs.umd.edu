---
layout: post
title:  "Visual Discovery of Communication Patterns in Email Networks"
tags: [news, research, visualization, gephi, email]
category: thoughts
image:
comments:
share:
paper: visual-discovery-email-networks.pdf
---

![Analysis of Betweenness Centrality]({{ site.url }}/images/gephi_analysis.png)

<p style="text-align:center; padding-bottom:18px;">
    <strong><small>An analysis of key players in a large and small email network.</small></strong>
</p>

In the age of social networks, it is very popular to analyze Twitter and Facebook to detect relationships and communities and to discover how information flows between groups. However, both Facebook and Twitter are examples of &ldquo;performed&rdquo; social networks: because a user has full control over who he or she &ldquo;friends&rdquo; or &ldquo;follows,&rdquo; he can artificially create a persona that he wishes the outside world to perceive him as. In fact, it has been well-studied that expressions of taste typically exclude the most critical groups to communication flows, and that social networks are merely the expressions of performed taste (Liu, 2007).

Email, on the other hand, is used in every facet of modern life and typically includes and aggregates facets of ourselves that would otherwise be segregated &mdash; for example, professional and personal personas. Because we spend so much time writing, reading, and responding to email, it has become integrated into a communication fabric that far exceeds other media that may get more analytical attention like text messaging or social networks. Email, therefore, can be seen to embed a natural communication network, from which we can extract rich insights about actual communications and influences within a single ego network &mdash; the email inbox of a single user &mdash; without the bias of taste performance.

In this paper, we will explore the visual analysis of two personal email networks, one from each of the authors, where one network is extremely large in terms of the number of nodes (email addresses) and the other network is extremely large in terms of the number of edges (cliquish, highly-connected email). We hope to show that if a user can visualize their own networks, they will better understand the dynamic nature of their communication in terms of key players, communities, and gaps in communication. Through such visual understanding, users may be better able to respond to and manage the constant flow of email messages meaningfully.

We will use Gephi (Gephi, 2010) to visualize email networks that have been extracted from `mbox` files via a Python tool written by the authors called `tribe`, which serializes the network into GraphML (Brandes, 2010). Gephi is widely used for visual exploration of networks (Bastian, 2009) and includes many features for the statistical analysis of small to medium networks (McSweeney, 2009). Work of note demonstrates the analysis of dynamic network connections within Twitter conversations (Bruns, 2012), which shows how Gephi might be used for networks whose topology can rapidly shift, as in email. As part of our analysis we will critique Gephi and contrast it to NodeXL (Smith, 2010), another popular tool for visual network analysis.


&hellip;

**Read the full article**: [Visual Discovery of Communication Patterns in Email Networks]({{ site.url }}/papers/{{ page.paper }})
