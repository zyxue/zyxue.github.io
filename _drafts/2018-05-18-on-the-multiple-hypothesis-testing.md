---
layout: post
title: On the multiple hypothesis testing
author: Zhuyi Xue
tags: linear algebra
---

During multiple hypothesis testing, based on the <a href="#">confusion
table, there are different ways to control the type I error rate, a
few common ones are

* Familywise error rate (FWER)
* False discovery rate (FDR)
* False discovery proportion (FDP)

A few concepts that are worth deeper understanding,

**$\alpha$ significance level**

It means different things when the metric under control is different

$$
\begin{align*}
\; & q_i > \frac{\alpha}{m_0}                     & \text{for all}\ i = 1, \cdots, m_0  & \;\;\;\;\; \text{Bonferroni inequality} \\
\; & q_i > \frac{i \alpha}{m_0 \sum_{j=1}^{m_0}}  & \text{for all}\ i = 1, \cdots, m_0  & \;\;\;\;\; \text{Hommel inequality} \\
\; & q_i > \frac{i \alpha}{m_0}                   & \text{for all}\ i = 1, \cdots, m_0  & \;\;\;\;\; \text{Sime's inequality}
\end{align*}
$$

Sime's inequality is under weaker the PDS condition.
