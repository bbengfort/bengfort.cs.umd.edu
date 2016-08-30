---
layout: post
title:  "Reading &ldquo;Metastorage: A Federated Cloud Storage System to Manage Consistency-Latency Tradeoffs&rdquo;"
#modified: 2016-08-30 10:41:08 -0400
tags: [metastorage, distributed systems, papers, cloud, nosql]
category: scientific-reading
comments:
share:
---

> <small>D. Bermbach, M. Klems, S. Tai, and M. Menzel, “Metastorage: A federated cloud storage system to manage consistency-latency tradeoffs,” in Cloud Computing (CLOUD), 2011 IEEE International Conference on, 2011, pp. 452–459.</small>

## Abstract ##

<small>Cost and scalability benefits of Cloud storage services are apparent. However, selecting a single storage service provider limits availability and scalability to the selected provider and may further cause a vendor lock-in effect. In this paper, we present MetaStorage, a federated Cloud storage system that can integrate diverse Cloud storage providers. MetaStorage is a highly available and scalable distributed hash table that replicates data on top of diverse storage services. MetaStorage reuses mechanisms from Amazon’s Dynamo for cross-provider replication and hence introduces a novel approach to manage consistency-latency tradeoffs by extending the traditional quorum (N,R,W) configurations to an (N<sub><small>P</small></sub>,R,W) scheme that includes different providers as an additional dimension. With MetaStorage, new means to control consistency-latency tradeoffs are introduced.</small>

## Critical Reading ##

The primary concern of Bermbach et al. seems to be vendor lock in to specific cloud services and the possibility of a single point of failure via downtimes or even going out of business. However, they also identify the fact that cloud services provide a variety of consistency and performance guarantees that have an impact on consistency due to latency. To solve these problems they present a system called MetaStorage which federates several cloud services with a focus on ensuring that MetaStorage is as available as the underlying service.

MetaStorage performs GET and PUT requests using an (N<sub><small>P</small></sub>,R,W) policy where N is specified not as the number of replica servers but rather the number of providers. Distribution occurs by a list of preferences based on the hash of the key (e.g. a DHT). MetaStorage maintains message queues with each provider and for requests submits the access to R or W replicas in order of the preference, continuing until enough replicas allow for a response. For GET in particular, if R identical responses come in, that response is returned and for any nodes that don't return that response, they are repaired with that value. If R reads cannot agree, all responses are returned or an error is returned with as many responses as available -- thus delegating to the client the handling of inconsistency. Hinted handoffs ensure that all providers receive writes even after a response has been sent.

Crucially, in order to ensure that there can be multiple distributors handling requests, some coordination is required -- particularly to ensure all nodes maintain the same preference list. Rather than choosing to use a consensus approach to coordination, MetaStorage implements a lightweight, semi-centralized approach by giving all coordinators a complete list of all other coordinators in the system. The master is assumed to be at the top of the list; if the master cannot be reached, it is popped off the list and the next coordinator is selected. As coordinators come online, they are appended to the bottom of the list. Because the list is ordered and as long as every coordinator knows of more than two other coordinators, every replica is guaranteed to choose the same master. Configuration changes then can be made by allowing the master to wait until all currently queued requests are complete, holding all distributor nodes while the configuration is applied, then resuming operation.

MetaStorage is certainly interesting as it is a first attempt to federate different consistency models; however MetaStorage can only guarantee eventual consistency both because the underlying storage mechanism is eventually consistent and because of the Dynamo-like policy they use for replication. Moreover while they cite an inconsistency window of 0.09ms (further reinforcing that eventual is fast enough) they show that the latency overhead is 300ms in a single availability region. They have performed no studies that discus geographic or wide area replication.
