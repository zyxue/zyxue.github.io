---
layout: post
title:  Google Cloud Platform vs HPC
author: Zhuyi Xue
tags: google cloud platform,GCP,HPC
---

Recently, I have enjoyed using Google Cloud Platform (GCP) quite a bit. I have
also been doing high throughput computing for the last 5 years. Here I
summarize the major difference between the Google Cloud Platform and a typical
HPC environment. In my opinion, Google Cloud Platform is much better and more
flexible to use regardless of cost. I don't have a concrete idea on how much
HPC setup and maintainence costs, but GCP to me is

1;95;0c[not cheap](https://cloud.google.com/products/calculator/).

| |GCP| In-house HPC |
|:-|:-----|:--------------|
|**Storage**| A varietyof kinds for different needs, e.g. [Google Cloud Storage](https://cloud.google.com/storage/docs/overview), [Google Cloud SQL](https://cloud.google.com/sql/docs/introduction), [Google datastore](https://cloud.google.com/datastore/docs/concepts/overview), [Google Block storage](https://cloud.google.com/compute/docs/disks/), etc.|[GPFS](https://en.wikipedia.org/wiki/IBM_General_Parallel_File_System)|
|**Node/Instance**|	[A variety of CPU, memory sizes, or even customized](https://cloud.google.com/compute/docs/machine-types)|Fixed|
|**Network**|Not sure, ~180ms latency based on a [2013 test](https://blog.rescale.com/mpi-latency-on-google-compute-engine/)|[InfiniBand](https://en.wikipedia.org/wiki/InfiniBand)|
|**Queuing time**| No|Could be long|
|**Operating system**| [A variety of OS](https://cloud.google.com/compute/docs/operating-systems/), including standard linux distros, so software installation is relatively standard and easy|Not necessarily standard linux, could be specialized, no root access, software installation can be tricky|
|**Docker Support**|✔|uncommon|
|**Horizontal scale**|You can secure thousands of CPUs easily| Depending on the actual usage policy, usually it's not as flexible|
|**On-demand**|Yes, a cluster can be torn down easily after the analysis is done| No, the cluster is always there, busy or idle|
|**Cluster type**|Besides a normal setup of a collection of nodes, you could also set up [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/) type of cluster, [Google Cloud Dataproc](https://cloud.google.com/dataproc/overview), which will be useful for using frameworks like [ADAM](http://bdgenomics.org/) in the future |Hadoop or Spark cluster setup is still not very common|
