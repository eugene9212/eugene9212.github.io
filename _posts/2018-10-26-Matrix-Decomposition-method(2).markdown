---
disqus_shortname: eugene
layout: post
title:  "Matrix Decomposition _ (2) Cholesky Decomposition"
date:   2018-10-26
author: Eugene Lee
categories: "Statistical-Computing"
mathjax: true
---

# Matrix Decomposition
## Chapter 2. Cholesky Decomposition
### Basic Idea
Transform A into $LL^{T}$ form. (L refers to lower triangular matrix)

### Example (How it works)
$$
A=\begin{bmatrix} 4 & 2 & 2 & 4 \\
 2 & 5 & 7 & 0 \\
 2 & 7 & 19 & 11 \\
 4 & 0 & 11 & 25 \\\end{bmatrix}
$$

From now on, we will going to find L with following steps.

Stap by step we should look A in (1 by 1) (2 by 2) (3 by 3) (4 by 4) matrix.

- (1) Consider 4, which is (1,1) element of A, as small A. Find $L_{11}$

$4=L_{11}^{2}.$

${\color{blue} \rightarrow L_{11} = 2}$ 

(Here, you do not need to consider sign of the element L)

- (2) Consider $\begin{bmatrix} 4 & 2 \\ 2 & 5 \\\end{bmatrix}$. Now,

$$
\begin{align}
\begin{bmatrix} 4 & 2 \\ 2 & 5 \\\end{bmatrix}
&=
\begin{bmatrix} 2 & 0 \\ L_{21} & L_{22} \\\end{bmatrix}
\begin{bmatrix} 2 & L_{21} \\ 0 & L_{22} \\\end{bmatrix}\\
&=\begin{bmatrix} 4 & 2 L_{21} \\ 2 L_{21} & L_{21}^2 + L_{22}^2 \\\end{bmatrix}
\end{align}
$$

${\color{blue} \rightarrow L_{21} = 1, L_{22} = 2}$ 

- (3) Consider $\begin{bmatrix} 4 & 2 \\ 2 & 5 \\\end{bmatrix}$. Now,




### Additional Knowledge

### Implementation in R


The following code is R code, working as Gaussian Elimination.

```

```
