---
layout: post
title:  "Matrix Decomposition _ (1) Gaussian Elimination"
date:   2018-10-23
author: Eugene Lee
categories: "Statistical Computing"
mathjax: true
---

# Matrix Decomposition
### Before we begin (Motivation)
Creating a matrix in computing you have to consider several problems, such as storage matter, computing complexity and inverting issue. Speaking of inverting issue, you would rather use other methods than just invert the matrix using built-in function, for example 'solve' in R.

In this topic, we'll going to learn three methods, Gaussian Elimination, Cholesky Decomposition, and QR decomposition, that can replace inverting and lessen your computation complexity.
(If you want to get into the bottom line, just skip the following and go straight to Chapter 1)

Before we start specific method, let's take a look at an example(linear equation) form which needs inverting.

$$
Ax = y
$$

If we want to solve this equation, $x = A^{-1}y$ might work. And depending on the shape of A, invert matter becomes much easier. Now, let's think about different forms of A. 

- A = diagonal matrix
- 
$$
\begin{bmatrix}a_{11} & 0 & 0  \\ 0 & a_{22} & 0  \\ 0 & 0 & a_{33}  \\\end{bmatrix}
\begin{bmatrix} x_{1}  \\ x_{2}  \\ x_{3}  \\\end{bmatrix}
=\begin{bmatrix} b_{1}  \\ b_{2}  \\ b_{3}  \\\end{bmatrix}

\Leftrightarrow \begin{bmatrix} a_{11}x_{1}=b_{1} \\ a_{22}x_{2}=b_{2}  \\ a_{33}x_{3}=b_{3}  \\\end{bmatrix}
$$

$$
\therefore x_{1}=\frac{b_{1}}{a_{11}}, x_{2}=\frac{b_{2}}{a_{22}}, x_{3}=\frac{b_{3}}{a_{33}}
$$

As you can see above, if A is a diagonal matrix, you don't really need to invert A. Just solve the equation element-wise.

- A = low-triangular matrix
$$
\begin{bmatrix}a_{11} & 0 & 0  \\ a_{21} & a_{22} & 0  \\ a_{31} & a_{32} & a_{33}  \\\end{bmatrix}
\begin{bmatrix} x_{1}  \\ x_{2}  \\ x_{3}  \\\end{bmatrix}
=\begin{bmatrix} b_{1}  \\ b_{2}  \\ b_{3}  \\\end{bmatrix}

\Leftrightarrow \begin{bmatrix} a_{11}x_{1}=b_{1} \\ a_{21}x_{1} + a_{22}x_{2}=b_{2}  \\ a_{31}x_{3} + a_{32}x_{2} + a_{33}x_{3}=b_{3}  \\\end{bmatrix}
$$

$$
\therefore x_{1}=\frac{b_{1}}{a_{11}}, 
x_{2}=\frac{1}{a_{22}}(b_{2}-\frac{a_{21}b_{1}}{a_{11}}), 
x_{3}=\frac{1}{a_{33}}\big(-\frac{a_{32}{a_{22}}}(b2-\frac{a_{21}b_{1}}{a_{11}}-\frac{a_{31}b_{1}}{a_{11}}) \big)
$$


As you can see above, special structrue of A does not require inverting. This is the reason why we use matrix factorization(decomposition), such as LU, SVD, and so on. After decomposition, solving linear equation becomes much easier, and lower the computation.

To sum up, instead of inverting the matrix, decomposing the matrix into the special structure outperforms in computation complexity, esepcially when the computation size gets bigger.

- - -

## Chapter 1. Gaussian Elimination
### Basic IDea
$$
BAx = Bb
$$

: multiply appropriate matrix B on both sides of linear equation.
(* This matrix B will make matrix A a special form like upper triangular form.)

### How it works
$$
A=\begin{bmatrix} a_{11} & a_{12} & a_{13} \\
 a_{21} & a_{22} & a_{23} \\
 a_{31} & a_{32} & a_{33} \\\end{bmatrix}
$$

multiplying a matrix, you'll going to make A upper triangular column by column.

(column1) Remain (1,1) element of A and remove all other elements in the first column of A.


### Implementation in R
```
backsolve(BA, Bb)
```
