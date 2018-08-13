---
layout: post
title: Comparison of Storage options on Google Cloud Platform
author: Zhuyi Xue
tags: Google Cloud Platform
---

Here is my summary of the comparison among different storage options available
on [Google Cloud Platform](https://cloud.google.com/) (GCP).

|                       | SQL or NoSQL | (Could be) Transactional                    | Scalable | Low latency |
|-----------------------|--------------|---------------------------------------------|----------|-------------|
| Bigtable (HBase)      | NoSQL        | ✕                                           | ✓        | ✓           |
| Datastore (megastore) | NoSQL        | ✓                                           | ✓        |             |
| Cloud SQL             | SQL          | ✓                                           | ✕        |             |
| Spanner               | SQL          | ✓ (High transactions)                       | ✓        |             |


<br>

In addition, another storage option, BigQuery, is SQL-like and features in
interactive analysis of large-scale (up to petabyte-scale) dataset.

Reference: https://cloud.google.com/storage-options/
