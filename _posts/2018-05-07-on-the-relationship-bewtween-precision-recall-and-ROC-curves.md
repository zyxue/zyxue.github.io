---
layout: post
title: On the Relationship Between Precision-Recall and ROC Curves
author: Zhuyi Xue
tags: statistics, p-value, FDR
---

A ROC curve plots the relationship between true positive rate (TPR, i.e.
sensitivity/recall) and false positive rate (FPR, i.e. 1 - specificity), while a
precision-recall (PR) curve plots the relationship between positive predictive
value (PPV, i.e. precision) and recall (TPR).

For a tabular view of these metrics, please see the confusion table at
https://en.wikipedia.org/wiki/Sensitivity_and_specificity.

Today, I reread a very insightful paper, **The Relationship Between
Precision-Recall and ROC Curves**
([pdf](https://www.biostat.wisc.edu/~page/rocpr.pdf)), by Jesse Davis and Mark
Goadrich, published on 2006, and here I summarize my understanding of the key
take-home messages.

1. When the dataset is highly skewed in the class distribution, use PR curve as
   ROC curve is deceiving as a proxy for the performance of the classifier or
   test that produce the result, i.e. the confusion table.
1. For any dataset and a given algorithm, the points on the ROC curve and those
   on the PR curve have one-to-one correspondences. In otherwords, each (TPR,
   FPR) point has a correspondent (PPV, TPR) point.
1. If a curve dominates in the ROC space, then its corresponding curve in the PR
   space also dominates.
1. A convex ROC hull in the ROC space corresponds to a so-called achievable PR
   curve in the PR space, and they discard the exact same points ((TPR,
   FPR) in ROC space, and (PPV, TPR) in the PR space)
1. While linear interpolation is valid in the ROC space, it overestimates the
   AUC in the PR space, thus insufficient.
1. An algorithm that optimizes AUROC is not guaranteed to optimize AUPRC.
