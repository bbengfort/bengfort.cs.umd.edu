---
layout: post
title:  "Computing a Bayesian Estimate of Star Rating Means"
tags: [python, bayesian methods, recommendations]
category: tutorials
comments:
share:
---

Consumers rely on the collective intelligence of other consumers to protect themselves from coffee pots that break at the first sign of water, eating bad food at the wrong restaurant, and stunning flops at the theater. Although occasionally there are metrics like Rotten Tomatoes, we primarily prejudge products we would like to consume through a simple 5 star rating. This methodology is powerful, because not only does it provide a simple, easily understandable metric, but people are generally willing to reliably cast a vote by clicking a star rating without too much angst.

In aggregate, this is wonderful. Star ratings can minimize individual preference because the scale is not large enough to be too nuanced. After enough ratings, it becomes pretty clear whether or not something is a hit or a flop. The problem is, however, that many ratings are needed to make this system work because the quality of a 5 star rating depends not only on the average number of stars but also on the number of reviews.

Consider the following two items:

1. Presto Coffee Pot - average rating of 5 (1 review).
2. Cuisinart Brew Central - average rating of 4.1 (78 reviews).

Which item should be listed first in a list of items sorted by rating? It would seem clear that the Brew Central has more reviewers and that many of them are happy with the product. Therefore, it should probably appear before the Presto even though its average rating is lower.

We could brainstorm a couple of strategies that would give us this outcome and would help us deal with items that have an insufficient number of ratings when we come across them in the future:

- We could simply list the number of reviews in addition to the average rating. However, this doesn't help with sorting the products.
- We could set a floor of n number of ratings before a product is shown, but this decreases the likelihood that a product will be reviewed because it remains unseen. Not only that, but how do we choose n?

Instead, we need some empirical metric for the average rating that embeds the number of reviewers into the score. Most methodologies in use today use Bayesian estimation, which takes into account the fact that we don't have enough data to make an estimation via the mean and also incorporates all the data we have about other observations.

In this post, we will consider how to implement a Bayesian estimation of the mean of star reviews with a limited number of observations. The estimation is useful for recommender services and other predictive algorithms that use preference space measures like star reviews. It is also a good crash course in Bayesian estimation in general for those that are interested.

> [Read the complete article at District Data Labs](https://districtdatalabs.silvrback.com/computing-a-bayesian-estimate-of-star-rating-means)
