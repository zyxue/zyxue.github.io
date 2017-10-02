---
layout: post
<!-- title:   -->
author: Zhuyi Xue
tags: linear algebra
---

# Matrix factorization

1. Gauss-Jordan elimination, for calculating inverse, leads to $$A = LU$$
1. Gram-Schmidt transformation, for finding orthonormal bases, leads to $$A = QR$$

# Three ways for calculating determinants

1. Multiply the $n$ pivots, the pivot formula
1. Add up $$n!$$ terms, the big formula
1. Combine $n$ smaller determinants, the cofactor formula


# Properties of determinants

1. The determinant of the $$n \times n$$ matrix is 1
1. The determinant changes sign when two rows are exchanged
1. The determinant is a linear function of each row separately
1. If two rows of $$A$$ are equal, the $$\mathrm{det} A = 0$$
1. Subtracting a multiple of one row from another row leaves determinant unchanged
1. A matrix with a row of zeros has $$\mathrm{det} A = 0$$
1. If $$A$$ is triangular, then $$\mathrm{det} A = a_{11} a_{22} \cdots a_{nn}$$, the product of diagonal entries
1. If $$A$$ is singular, then $$\mathrm{det} A =0$$. If A is invertible, then $$\mathrm{A} \neq 0$$
1. The determinant of $$AB$$ is the $$\mathrm{det} A$$ times $$\mathrm{det} B$$: $$\vert AB \vert = \vert A \vert \vert B \vert$$
1. Transpose does not change determinant: $$\vert A \vert = \vert A^T \vert$$
