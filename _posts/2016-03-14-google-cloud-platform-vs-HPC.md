---
layout: post
title:  Google Cloud Platform vs HPC
author: Zhuyi Xue
tags: google cloud platform,GCP,HPC
---

Recently, I have enjoyed using Google Cloud Platform (GCP) a lot. I have also
been doing high throughput computing for the last 5 years. Here I summarize the
major difference between the Google Cloud Platform and a typical HPC
environment. In my opinion, Google Cloud Platform is much better and flexible
to use regardless of cost. I don't have a concrete idea on how much HPC setup
and maintainence costs either, but GCP to me is not cheap.

| |GCP| In-house HPC |
|:-|:-----|:--------------|
|**Storage**| A varietyof kinds for different needs, e.g. Google Cloud Storage, SQL, Googledatastore(NoSQL), Block storage, etc.|GPFS|
|**Node/Instance**|	A varity of CPU, memory sizes, or even customized|Fixed|
|**Network**|Not sure, ~180ms latency based on a [2013 test](https://blog.rescale.com/mpi-latency-on-google-compute-engine/)|InfiniBand|
|**Queuing time**| No|Could be long|
|**Operating system**| A variety of OS,including standard linux distros, so software installation is relatively standard and easy|Not necessarily standard linux, could be specialized, no root access, software installation can be tricky|
|**Docker Support**|✔|uncommon|
|**Horizontal scale**|You can secure thousands of CPUs easily| Depending on the actual usage policy, usually it's not as flexible|
|**On-demand**|Yes, a cluster can be torn down easily after the analysis is done| No, the cluster is always there, busy or idle|
|**Cluster type**|Besides a normal setup of a collection of nodes, you could also set up Hadoop, Spark type of cluster, which will be useful for using frameworks like ADAM (Big Data Genomics) in the future |Hadoop or Spark cluster setupis still not very common|
