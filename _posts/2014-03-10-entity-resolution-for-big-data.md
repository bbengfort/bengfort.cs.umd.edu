---
layout: post
title: "Entity Resolution for Big Data"
modified: 2014-03-10 09:57:07 -0400
tags: [entity resolution, big data, calustering]
category: tutorials
image:
    feature: big_data_er_slice.png
    credit: Anja Jentzsch (Wikimedia Commons)
    creditlink: http://en.wikipedia.org/wiki/Linked_data
comments:
share:
---

Entity Resolution is becoming an important discipline in Computer Science
and in Big Data especially with the recent release of Google's
[Knowledge Graph][knowledge_graph] and the open [Freebase API][freebase].
Therefore it is exceptionally timely that last year at KDD 2013, Dr. Lise
Getoor of the University of Maryland and Dr. Ashwin Machanavajjhala of
Duke University gave a tutorial on
[_Entity Resolution for Big Data_][kdd_tutorial]. We were fortunate enough
to be invited to attend a run through workshop at the Center for Scientific
Computation and Mathematical Modeling at College Park, and wanted to
highlight some of the key points for those unable to attend. This post has
been reposted from other sources.

<style>
    article img {
        margin: 28px auto;
        padding: 0;
        text-align: center;
    }
    a.image-popup {
        width: 100%;
    }
</style>

<!--more-->

## What is Entity Resolution? ##

Entity Resolution is the task of disambiguating manifestations of real
world entities in various records or mentions by linking and grouping. For
example, there could be different ways of addressing the same person in
text, different addresses for businesses, or photos of a particular object.
This clearly has many applications, particularly in government and public
health data, web search, comparison shoppping, law enforcement, and more.

Additionally, as the volume and velocity of data grows, inference across
networks and semantic relationships between entities becomes a greater
challenge. Entity Resolution can reduce the complexity by proposing
canonicalized references to particular entities and deduplicating and
linking entities. As an example, consider the following example of a
coauthor network from bibliographic data used for InfoVis 2004.

[![ER and Network Analysis][network_resolution]][network_resolution]

Deduplication significantly reduced the complexity of the network from an
ninth order graph to a much simpler fourth order graph, of significantly
less size. Dr. Getoor's work on [D-Dupe, an interactive data deduplication tool,][ddupe]
demonstrates how an iterative entity resolution process could work for
social networks, an increasingly important and growing data set that seems
to trancend traditional stovepipes like Twitter and Facebook. Yet another
example is the multiport disambiguation of Traceroute output of routers,
something that when incorrectly analyzed led to the commentary of some
technologists about the fagility of the Internet- when in fact most
Internet backbones are well defined and hardened.

However, there are significant challenges to the ER discipline, least of
which is the fact that there is no unified theory and ironically, ER
itself goes by many names! Other challenges like language ambiguity, poor
data entry, missing values, changing attributes and formatting, as well as
abbreviations and truncation mean that ER is a discpline that includes not
just databases and information retrieval, but also natural language
processing and machine learning.

Scaling to big data just increases the challenge, as the need for
heterogeneity and cross-domain resolution become important features. As a
result, ER techniques must be parallel and efficient, in order to be used
in the context of Big Data techniques like MapReduce and distributed graph
databases.

## Tasks in Entity Resolution ##

Generically speaking we can discuss ER as follows: there exists in the real
world entities, and in the digital world, records and mentions of those
entities. The records and mentions may take many forms, but they all refer
to only a single real world entity. We can therefore discuss the ER problem
as one involving matching record pairs corresponding to the same entity,
and as a graph of related records/mentions to related entities.

[![Abstract ER Problem][problem_statement]][problem_statement]

**Deduplication**: the task of clustering the records or mentions that
correspond to the same entity. There is an intensional variant of this
task: to then compute the cluster representative for each entity. This is
commonly what we think of when we consider Entity Resolution.

**Record Linkage**: a slightly different version of the task is to match
records from one deduplicated data store to another. This task is proposed
in the context of already normalized data particularly in relational
databases. However, in the context of Big Data, a one to one comparison of
every record is not optimal.

**Reference Matching**: in this task, we must match noisy records to clean
ones in a deduplicated reference table. Note that is assumed that two
identical records are matches, this task is related to the task of entity
disambiguation.

Further, when relationships between entites are added, each of the three
problem statements must also take into account the relationships between
records/mentions. Entity resolution techniques must take into account these
relationships as they are significant to disambiguation of entities.

### Evaluation of Entity Resolution ###

A quick note on the evaluation of entity resolution. For _pairwise metrics_
we consider Precision and Recall (e.g. F1 scores), as well as the
cardinality of the number of predicted matching pairs. _Cluster level metrics_
take into account purity, completeness, and complexity. This includes
cluster-level precision and recall, closest cluster, MUC, Rand Index, etc.
However, there has been little work on the correct prediction of links.

## The Algorithms of Entity Resolution ##

This section includes a brief overview of algorithmic basis proposed by
Lise and Ashwin to provide a context for the current state of the art of
Entity Resolution. In particular, they discussed Data Preparation, Pairwise
Matching, Algoritms in Record Linkage, Deduplication, and Canonicalization.
They also considered collective entity resolution algorithms, that I will
briefly mention. Of course, they went into more depth on this section than
I will, but I hope to provide a good overview.

### Data Preparation ###

First the tasks of _schema and data normalization_ are required preparation
for any Entity Resolution Algorithms. In this task, schema attributes are
matched (e.g. contact number vs phone number), and compound attributes like
addresses are normalized. Data normalization involves converting all
strings to upper or lower case and removing whitespace. Data cleaning and
dictionary lookups are also important.

> Initial data prep is a big part of the work; smart normalization can go
> a long way!

The goal is to construct, for a pair of records, a "comparison vector" of
_similarity scores_ of each component attribute. Similarity scores can
simply be Boolean (match or non-match) or they can be real values with
distance functions. For example, Edit distance on textual attributes can
handle typographic errors. Jaccard coefficients and other distance metrics
can be used to compare sets. Even phonetic similarity can be used.

### Pairwise Matching ###

After we have constructed a vector of component-wise similarities for a
pair of records, we must compute the probability that the pair of records
is a match. There are several methods for discovering the probability of a
match. Two simple proposals are to use a weighted sum or average of
component similarity scores, and use thresholds. However, it is extremely
hard to pick weights or tune thresholds. Another simple approach can be
rule based matching, but manual formulation of rule sets is difficult.

One interesting technique is the Fellegi &amp; Sunter Model: given a record
pair, `r = (x,y)` the comparison vector, is &gamma;. If `M` is the set of
matching pairs of records and `U` is the set of non-matching pairs of
records, linkage decisions are based on the probability of &gamma; given `r`
&#8715; `M` divided by the probability of &gamma; given `r` &#8715; `U`.
Further, you can decide if a record is a match or not based on error bounds,
&mu; and &lambda; that create thresholds for whether a record is a match,
a non-match, or it is simply uncertain.

In practice, Fellegi &amp; Sunter requires some knowledge of matches to
train the error bounds, therefore some form of supervised learning is
required. In general, there are severaal techniques for Machine Learning
algorithms that can be applied to ER - Decision Trees, SVNS, Ensembles of
Classifiers, Conditional Random Fields, etc. The issue is Training Set
Generation since there is an imbalance of classes: non-matches far
outnumber matches, and this can result in misclassification.

Active Learning methods and Unsupervised/Semi-supervised techniques, are
used to attempt to circumvent the difficulties of traning set generation.
Lise and Ashwin propose Committee of Classifiers approaches and even
crowdsourcing to leverage active learning to build a training set.

### Constraints ###

There are several important forms of constraints that are relevant to the
next sections, given a specific mention, Mi:

1. **Transitivity**: If M1 and M2 match, M2 and M3 match, then M1 and M3
   must also match.
2. **Exclusivity**: If M1 matches with M2, then M3 cannot match with M2
3. **Functional Dependency**: If M1 and M2 match, then M3 and M4 must match.

For these broad classes of constraints there are positive and negative
evidences of specific constraints, e.g. there is a converse to the match
constraint for a non-match or vice-versa. You may even consider hard and
soft constraints for the constraint types, as well as extent: can the
constraint be applied globablly or locally?

Based on these constraints, you can see that transitivity is the key to
deduplication. Exclusivity is the key to record linkage and functional
dependencies are used for data cleaning. Further constraints like
aggregate, subsumption, neighboord, etc. can be used in a domain specific
context.

### Specific Algorithms for Problem Domains ###

**Record Linkage**: propagation through the exclusitivity constraint means
that the current best solution is _Weighted K-Partite Matching_. Edges are
pairs between records from different data sets whose weights are the
pairwise match score. The general problem is NP-hard, therefore the common
optimization is to perform successive bipartite matching.

**Deduplication**: propegation through transitivity leads to a clustering
based entity resolution. There are a variety of clustering algorithms, but
the common input is pairwise similarity graphs. Many clustering algorithms
may also require the construction of a _cluster representative_ or a
_canonical entity_. Although heirarhcical clustering or nearest neighbor
can be used, the recommended approach is _Correlation Clustering_.

Correlation Clustering uses Integer Linear Programming to maximize a cost
function that places positive and negative benefits of clustering mentions
x,y together, such that the Transitive closure is satisfied. However,
solving ILP is NP-hard, therefore a number of heuristics are used to
approximate the cost function including Greedy BEST/FIRST/VOTE algorithms,
the Greedy PIVOT algorithm, and local search.

**Canonicalization**: Selection of the mention or cluster representative
that contains the most information. This can be rule based (e.g. longest
length), or for set value attributes a UNION. Edit distance can be used
to determine a most representative centroid, or use "majority rule". Other
approaches include the Stanford Entity Resolution Framework (blackbox).

### Collective Approaches ###

If the decision for cluster-membership depends on other clusters, we can
use a collective approach. Collective approaches include non-probabilisitic
approaches like similarity propegation, or probabilistic models including
generative frameworks, or simply a hybrid approach.

Generative probabilistic approaches are based on directed models, where
dependecies match decisions in a generative manner. There are a variety of
approaches, notably based on LDA and Bayesian Networks. Undirected
probabilistic approaches use semantics based on Markov Networks, to the
advantage that this allows a declarative syntax based on first-order logic
to create constraints. Several suggested approaches include Conditional
Random Fields, Markov Logic Networks (MLNs), and Probabilistic Soft Logic.

Probabilistic soft logic introduces _reverse predicate equivalence_ meaning
that the same relation with the same entity gives evidence of two entities
being the same. This is not true logically, but can allow us to predict
the probability of a match with truth values in [0,1]. The declarative
language is used to define a constrained continuous Markov random field
in first order logic, and we can use relaxed logical operators. This has
significant implication for scaling improvements.

## Scaling to Big Data ##

**Blocking/Canopy Generation**: The first approach to scaling to big data
is to use a blocking approach. Consider a Na&iuml;ve pariwise comparsion
with 1,000 business mentions from 1,000 citties. This is _1 trillion_
comparisons, which is 11.6 days for a microsecond comparison! However, we
know that business mentions are probably city-specific, therefore only
comparing mentions within cities reduces the comparison space to the more
managable _1 billion_ comparisions, which takes only 16 minutes at the same
rate.

[![Blocking Heuristic][blocking]][blocking]

Although some matches can be missed using blocking techniques, the key is
to select a blocking algorithm that minimizes the subtraction of the set
of matching pair records and those satisfying the blocking criterion as in
the Venn diagram above, and maximizes the intersection. Common blocking
algorithms include hash based blocking, similarity or neighborhood based
blocking, or creating complex blocking predicates by combining simple
blocking predicates. Another powerful technique is minHash or locality
sensitive hashing, which uses distance measures and performs very well, and
is recommended as the state of the art for blocking.

The final approach to blocking is more inline with the clustering methods
of earlier approaches. In _Canopy Clustering_ a distance metric is selected
with two thresholds. By picking a random mention, we create a canopy using
the first threshold and we remove all mentions where the distance from the
centroid is less than the second threshold. We continue this process so
long as the set M is not empty. This approach is interesting because
mentions can be included in more than one canopy, and therefore reduces the
chance that the blocking method excludes an actual match.

[![Canopy Clustering][canopy]][canopy]

**Distributed ER**: The MapReduce framework is very popular for large,
parallel tasks and there are several open source frameworks related to
Hadoop to apply to your application. MapReduce can be used with disjoint
blocking- e.g. in the Map Phase (per record computation) you compute the
blocks and associate with various reducers, and in the reduce phase (global
computation) you can perform the pairwise matching. Several other challenges
relating to implementations with MapReduce are discussed in the text and
can be reviewed during implementations of ER in MapReduce.

## Conclusion ##

Entity resolution is becoming an increasingly important task as linked data
grows, and the requirement for graph based reasoning extends beyond
theoritical applications. With the advent of big data computations, this
need has become even more prevalent. Much research has been done in the
area in seperately named fields, and the tutorial by Dr. Getoor and Dr.
Machanavajjhala succinctly highlight the current state of the art and gives
a general sense of future work.

Specifically, there needs to be a unified approach to the theory which can
give relational learning bounds. Since Entity Resolution is often part of
bigger inference applications, there needs to be a joint approach to
information extraction, and a characterization of how the success (or
errors) affects larger reasoning quality. Similarly, there is a need for
large, real-world datasets with ground truth to establish benchmarks for
performance.

## References ##

This has been a summary of the work done by Dr. Lise Getoor and Dr. Ashwin
Machanavajjhala for their Tutorial entitled _Entity Resolution for Big Data_,
accepted at KDD 2013 in Chicago, Il.

You can find the complete tutorial slides at:
[http://www.cs.umd.edu/~getoor/Tutorials/ER_KDD2013.pdf][slides]

Special thanks to Lise and Ashwin for hosting Cobrain during their
run-through of the presentation.

[big_data_er]: {{ site.url }}/images/big_data_er.png "Challenges in Big Data for ER (Massive Graph)"
[knowledge_graph]: http://www.google.com/insidesearch/features/search/knowledge.html "Google's Knowledge Graph"
[freebase]: http://www.freebase.com/ "The Freebase API"
[kdd_tutorial]: http://www.kdd.org/kdd2013/accepted-tutorials "Accepted Tutorials at KDD 2013"
[network_resolution]: {{ site.url }}/images/network_resolution.png "Network Resolution"
[ddupe]: http://www.cs.umd.edu/projects/linqs/ddupe/ "D-Dupe: Interactive Data Deduplication"
[problem_statement]: {{ site.url }}/images/problem_statement.png "Abstract ER Problem"
[blocking]: {{ site.url }}/images/blocking.png "Blocking Heuristic"
[canopy]: {{ site.url }}/images/canopy.png "Canopy Clustering"
[slides]: http://www.cs.umd.edu/~getoor/Tutorials/ER_KDD2013.pdf "Tutorial Slides"
