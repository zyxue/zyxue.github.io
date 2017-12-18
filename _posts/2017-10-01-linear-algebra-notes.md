---
layout: post
<!-- title:   -->
author: Zhuyi Xue
tags: linear algebra
---

# Matrix factorization

1. Gauss-Jordan elimination, for calculating inverse, leads to $$A = LU$$
1. Gram-Schmidt transformation, for finding orthonormal bases, leads to $$A = QR$$
3. Via eigenvalues and eigenvectors: $$A = S \Lambda S^{-1}$$, where
   $$S$$ is the matrix of eigenvectors and $$\Lambda$$ is a diagonal
   matrix of eigenvalues

# Three ways for calculating determinants

1. Multiply the $$n$$ pivots, the pivot formula. (Pivots are just from elimination, no scaling)
1. Add up $$n!$$ terms, the big formula. The sum of $$n!$$ simple
   determinants: the product of items in a matrix which is formed by
   picking an iterm from each row and column of the original matrix,
   and times $$1$$ or $$-1$$ depending on the number of permutations
   contained in the column numbers following the row order.
1. Combine $$n$$ smaller determinants, the cofactor formula:
   $$\mathrm{det} A = a_{11}C_{11} + a_{12}C_{12} + \cdots +
   a_{1n}C_{1n}$$. Note that only one row is involved! More generally,
   $$\mathrm{det} A = a_{i1}C_{i1} + a_{i2}C_{i2} + \cdots +
   a_{in}C_{in}$$, where $$C_{ij} = (-1)^{i + j} \mathrm{det}
   M_{ij}$$, so the cofactor has to include its correct sign!


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

When the first three are satisfied, the others follow.

# Eigenvalue and eigenvectors

1. A matrix cannot be diagonalized without $$n$$ independent eigenvectors
1. Real matrixes can easily have complex eigenvalues and eigenvectors


# Linking pivots, determinants, eigenvalues, and least squares

When a symmetric matrix has any of the following five properties , it
has all of them:

1. All $$n$$ **pivots** are positive
1. All $$n$$ **upper left determinants** are positive
1. All $$n$$ **eigenvalues** are positive
1. All $$x^TAx$$ is positive except at $$x=0$$. This is the energy-based definition
1. $$A$$ equals $$R^T R$$ for a matrix $$R$$ with independent columns

_Reference_: Introduction to Linear Algebra (4th edition, 2009) by
Gilbert Strang.
