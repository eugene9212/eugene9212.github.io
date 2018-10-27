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
A=\begin{bmatrix} 4 & 2 & 2 \\
 2 & 5 & 7 \\
 2 & 7 & 19 \\\end{bmatrix}
$$

From now on, we will going to find L with following steps.

Stap by step we should look A in (1 by 1) (2 by 2) (3 by 3)matrix.

- (1) (1 by 1) sub matrix of A. Find $L_{11}$

$4=L_{11}^{2}.$

${\color{blue} \rightarrow} {\color{blue} L_{11} = 2}$

(Here, you do not need to consider sign of the element L)

- (2) (2 by 2) sub matrix of A. Find $L_{21}$ and $L_{22}$

$$
\begin{align}
\begin{bmatrix} 4 & 2 \\ 2 & 5 \\\end{bmatrix}
&=
\begin{bmatrix} 2 & 0 \\ L_{21} & L_{22} \\\end{bmatrix}
\begin{bmatrix} 2 & L_{21} \\ 0 & L_{22} \\\end{bmatrix}\\
&=\begin{bmatrix} 4 & 2 L_{21} \\ 2 L_{21} & L_{21}^2 + L_{22}^2 \\\end{bmatrix}
\end{align}
$$

${\color{blue} \rightarrow L_\{21\} = 1, L_\{22\} = 2}$

- (3) (3 by 3) sub matrix of A. Find $L_{31}$, $L_{32}, and $L_{33}$

$$
\begin{align}
\begin{bmatrix} 4 & 2 & 2\\ 2 & 5 & 7\\ 2 & 7 & 19\\\end{bmatrix}
&=
\begin{bmatrix} 2 & 0 & 0 \\ 1 & 2 & 0 \\ L_{31} & $L_{32} & $L_{33} \\\end{bmatrix}
\begin{bmatrix} 2 & 1 & L_{31} \\ 0 & 2 & & L_{32} \\ 0 & 0 & L_{33}\end{bmatrix}\\
&=\begin{bmatrix} 4 & 2 & 2L_{31} \\ 2 & 4 & L_{31} + 2L_{32} \\ 2 L_{31} & L_{31}^2 + 2 L_{32}^2 & L_{31}^2 + L_{32}^2 + L_{33}^2\end{bmatrix}
\end{align}
$$

${\color{blue} \rightarrow L_\{31\} = 1, L_\{32\} = 3}, L_\{33\} = 3}$

- To sum up, 
$$
\begin{bmatrix} A^{[k-1]} & a^{[k]} \\
 (a^{[k]})^{T} & a_{kk} \\\end{bmatrix}
=
\begin{bmatrix} L^{[k-1]} & 0 \\
 (l^{[k]})^{T} & l_{kk} \\\end{bmatrix}
\begin{bmatrix} L^{[k-1]} & (l^{[k]})^{T} \\
0 & l_{kk} \\\end{bmatrix}
$$

And each step, $l^{[k]}$ and $l_{kk}$ are unkowns.

Just finding $L_{n1}$ ~ $L_{nn}$ (last row of the matrix in each step) in each dtep, you will find the entire L, which construct $A=LL^{T}$

### Example in real world
$$
X^{T}A^{-1}X
$$

In Statistics, A is often an inverse of a variance. This can be a common form by extending $\big(\frac{\hat{\beta}}{\sqrt(se(\hat{\beta}))}\big)^2$ into a multivariate version.

To solve this,

- (step 1) Cholesky Decomposition
$$
A=LL^T
$$

- (step 2) Solve $Ly=x$   $\Leftrightarrow$    $y=L^{-1}x$
(Since L is low triangular, forward solve is possible.)

- (step 3) Solve X^{T}A^{-1}X
Then,
$$
x^{T}A^{-1}x &= x^{T}(LL^{T})^{-1}x
&= x^{T}(L^{T})^{-1}L^{-1}x
&= (L^{-1}x)^{T}L^{-1}x
&= y^{T}y
\end{align}
$$
$x^{T}A^{-1}x$
$$
A=LL^T
$$

### Implementation in R
Therefore, Cholesky Decomposition is solving,
$$
\begin{document}
\[
\begin{cases}
    L^{[k-1]}l^{[k]}=a^{[k]}\\
    l_{kk}^2 = a_{kk}-(l^{[k]})^{T}(l^{[k]})
\end{cases}
\]
\end{document}
$$


The following code is R code, working as Cholesky Decomposition.

```
fn.chol <- function(A)
{
  n <- ncol(A)
  
  # initalization
  Lk <- sqrt(A[1,1])
  
  # from 2nd column,
  k <- 2
  for (k in 2:n) {
    ak <- A[1:(k-1),k]
    l.dfk <- forwardsolve(Lk, ak) # for (k by others) element of L
    l.kk <- sqrt(A[k,k] - crossprod(l.dfk, l.dfk)) # for (k by k) element of L
    Lk <- rbind(cbind(Lk, rep(0, k-1)), c(l.dfk, l.kk))
    print(Lk)
  }
  L <- Lk
  
  return(L)  
}
```
