---
layout: post
title:  "Matrix Decomposition _ Method (1)"
date:   2018-10-23
author: Eugene Lee
categories: Statistical-Computing
---

# Matrix Decomposition
### Before we begin (Motivation)
Creating a matrix in computing you have to consider several problems, such as storage matter, computing complexity and inverting issue. Speaking of inverting issue, you would rather use other methods than just invert the matrix using function, for example 'solve' in R.

In this topic, we'll going to learn four methods that can replace inverting and lessen your computation complexity.
(If you want to get into the bottom line, just skip the following and go straight to Chapter 1)

Before we start specific method, let's take a look at an example form which needs inverting.

$$
Ax = y
$$

If we want to solve this equation, $$x = A^{-1}y$$ might work. And depending on the shape of A, invert matter becomes much easier. Now, let's think about different forms of A. 
- If A is a diagonal matrix, you don't really need to invert A. Just solve the equation element-wise.

$$
d_{1} x_{1} = y_{1}\\
d_{2} x_{2} = y_{2}\\
$$


$$
d_{1} x_{1} = y_{1} \\
d_{2} x_{2} = y_{2} \\
\cdots
$$

- If A is a low-triangular matrix, it becomes also easy.


$$
a_{11}x_{1} = y_1\\
a_{21}x_{1} + a_{22}x_{2} = y_{2}\\
\vdots\\
a_{k1}x_{1} + \cdots + a_{kk}x_{k} = y_{k}
$$

Simple form of A does not require inverting. This is the reason why we use matrix factorization(decomposition), such as LU, SVD, and so on. After decomposition, solving linear equation becomes much easier, and lower the computation.

To sum up, instead of inverting the matrix, decomposing the matrix into the special structure outperforms in computation complexity, esepcially when the computation size gets bigger.

- - -

## Chapter 1. Gaussian Elimination
### concept
$$
Ax = b\\
BAx = Bb
$$
- Idea : at the equation, multiplying B will transform A into a special structure(upper triangular form).


$$
\usepackage{amsmath}\[M=\begin{bmatrix}1 & 0 & 0  \\-\frac{a_{21}}{a_{11}} & 1 & 0  \\-\frac{a_{21}}{a_{11}} & 0 & 1  \\\end{bmatrix}\]
$$

### implementation in R
```
backsolve(BA, Bb)
```
