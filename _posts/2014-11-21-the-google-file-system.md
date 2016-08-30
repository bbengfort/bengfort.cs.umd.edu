---
layout: post
title:  "Reading &ldquo;The Google File System&rdquo;"
#modified:   2014-11-21 08:51:27 -0500
tags: [gfs, distributed systems, papers, google, hadoop]
category: scientific-reading
comments:
share:

---

> <small>S. Ghemawat, H. Gobioff, and S.-T. Leung, “The Google file system,” in ACM SIGOPS Operating Systems Review, 2003, vol. 37, pp. 29–43.</small>

## Abstract ##

<small>We have designed and implemented the Google File System, a scalable distributed file system for large distributed data-intensive applications. It provides fault tolerance while running on inexpensive commodity hardware, and it delivers high aggregate performance to a large number of clients.</small>

<small>While sharing many of the same goals as previous distributed file systems, our design has been driven by obser- vations of our application workloads and technological environment, both current and anticipated, that reflect a marked departure from some earlier file system assumptions. This has led us to reexamine traditional choices and explore radically different design points.</small>

<small>The file system has successfully met our storage needs. It is widely deployed within Google as the storage platform for the generation and processing of data used by our service as well as research and development efforts that require large data sets. The largest cluster to date provides hundreds of terabytes of storage across thousands of disks on over a thousand machines, and it is concurrently accessed by hundreds of clients.</small>

<small>In this paper, we present file system interface extensions designed to support distributed applications, discuss many aspects of our design, and report measurements from both micro-benchmarks and real world use.</small>

## Critical Reading ##

GFS is an application-optimized distributed file system that was designed with Google's requirements in mind. In particular, Google needed a replicated, highly-available system that prioritized streaming reads over random access and block writes or appends over random writes or overwrites. This makes perfect sense - Google's production systems are storing entire websites as a single file, which are written as the site is being crawled (in big blocks or via appends). Computing page rank or doing other tasks requires the extraction of links or parsing of an XML file, which uses streaming reads to fetch the data. Logging and other crucial disk bound operations at Google are also append-oriented.

The motto of Google's solution seems to be _simplicity_ - and for this reason a centralized master server is used to coordinate with chunk servers that reside on many commodity machines. The Master server only handles file meta data and acts as a traffic controller to clients who want data; it does not stand pose a bottle neck to data transfer. The chunk servers themselves are simply a software layer on top of the file system of the machine; meaning that data eventually will be passed to whatever filesystem exists under GFS (and Google did report some problems with the Linux file system particularly performance woes from `fsync` and `mmap`). Checksums for data integrity, chunkserver selection, garbage collection, recovery, heartbeats - all of the design choices that Google made in GFS is to simplify the distributed system for the types of applications that Google uses most often.

The result of this simplicity is the key innovation of this paper: the ability to use commodity hardware to provide production level distributed systems. If disks and machines are expected to fail, it is far better (and cheaper) to replace them with economically viable machines - allowing for _horizontal_ scaling; the more machines, the more capacity. There is no need for RAID or other expensive hardware because the filesystem replicates. And, extremely importantly - the data is where you will do the computation in the cluster, thus moving data off the disk to where the computation happens doesn't require network overhead.

It turns out that modern data appliances, especially those with terabytes of data, benefit from this distributed data storage model; especially when there is a distributed programming framework that also optimizes the storage model. Combined with another paper - _MapReduce: Simplified Data Processing on Large Clusters_ - GFS became the foundation for Big Data as we know it; and this paper was eventually implemented as HDFS in Hadoop. Chunks in the GFS are perfect inputs to Mapping processes, as each mapper can be run on every node in the cluster. Functional Mappers take a list of inputs as an argument and apply a function to every input value. In append-optimized systems chunks of data are therefore lists of inputs.

To the critique: there is a clear bottleneck in GFS, the Master server. This server has to be smart not only about chunk allocation, but also has to handle heartbeats, read and write requests and store metadata in memory. Although it does checkpoint itself to disk and have read-only shadow backups it is a central point of failure for the cluster. Worse, the master cannot handle many small files - it is optimized for files that are 64 MB or larger. Storing the meta data for billions of small files would eat up the memory on the server which can only vertically scale. Additionally because of the 64 MB chunks, there will be some storage loss for files that do not completely use up the chunk.
