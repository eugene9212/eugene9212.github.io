---
disqus_shortname: eugene
layout: post
title:  "Matrix Decomposition _ (1) Gaussian Elimination"
date:   2018-10-25
author: Eugene Lee
categories: "Statistical-Computing"
mathjax: true
---

# Matrix Decomposition
### Motivation
Before we begin, let's think about why matrix decomposition is critical in statistical computing. Creating a matrix in computing you have to consider several problems, such as storage matter, computing complexity and inverting issue. Speaking of inverting issue, rather than using built-in function which calculate the inverted matrix, you should seek for other methods to replace it because computation gets large when you invert the matrix.

Matrix Decomposition topic will handle three methods, Gaussian Elimination, Cholesky Decomposition, and QR decomposition, that are able to replace inverting and lessen your computation complexity.
(If you want to get into the bottom line, just skip the followings and go straight to Chapter 1)

Before we start a specific method, let's take a look at an example(linear equation) form which needs inverting.

$$
Ax = b
$$

If we want to solve this equation, $x = A^{-1}b$ might work. And depending on the shape of A, invert matter becomes much easier. Now, let's think about different forms of A.

- A = diagonal matrix

$$
\begin{bmatrix}a_{11} & 0 & 0  \\ 0 & a_{22} & 0  \\ 0 & 0 & a_{33}  \\\end{bmatrix}
\begin{bmatrix} x_{1}  \\ x_{2}  \\ x_{3}  \\\end{bmatrix}
=\begin{bmatrix} b_{1}  \\ b_{2}  \\ b_{3}  \\\end{bmatrix}

\Leftrightarrow \begin{bmatrix} a_{11}x_{1}=b_{1} \\ a_{22}x_{2}=b_{2}  \\ a_{33}x_{3}=b_{3}  \\\end{bmatrix}
$$

$$
\therefore x_{1}=\frac{b_{1}}{a_{11}},\;x_{2}=\frac{b_{2}}{a_{22}},\; x_{3}=\frac{b_{3}}{a_{33}}
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
\therefore x_{1}=\frac{b_{1}}{a_{11}},\;x_{2}=\frac{1}{a_{22}}(b_{2}-\frac{a_{21} b_{1}}{a_{11}}),\;x_{3}=\frac{1}{a_{33}}\Big( - \frac{a_{32}}{a_{22}}( b2 - \frac{a_{21} b_{1}}{a_{11}} ) -\frac{a_{31} b_{1}}{a_{11}} \Big)
$$


As you can see above, special structrue of A does not require inverting. This is the reason why we use matrix factorization(decomposition), such as LU, SVD, and so on. After decomposition, solving linear equation becomes much easier, and lower the computation.

To sum up, instead of inverting the matrix, decomposing the matrix into the special structure outperforms in computation complexity, esepcially when the computation size gets bigger.

- - -

## Chapter 1. Gaussian Elimination
### Basic Idea

$$
{\color{blue} B}Ax = {\color{blue} B}b
$$

: multiply appropriate matrix B on both sides of linear equation.

(* This matrix B will make matrix A a special form like upper triangular form.)

### Example
$$
A=\begin{bmatrix} a_{11} & a_{12} & a_{13} \\
 a_{21} & a_{22} & a_{23} \\
 a_{31} & a_{32} & a_{33} \\\end{bmatrix}
$$

From now on, we will going to make matrix B, with following steps.

By multiplying a matrix, you'll going to make A into upper triangular, column by column.

- (column1) Remain (1,1) element of A and remove all other elements in the first column of A. Following matrix make this possible. Then you can imagine that, by multiplying $(1,\;-\frac{a_{21}^{(1)}}{a_{11}^{(1)}},\;-\frac{a_{31}^{(1)}}{a_{11}^{(1)}})$ in front of A, we can make column 1 as we want.

$$
\begin{bmatrix} 1 & 0 & 0 \\
 -\frac{a_{21}^{(1)}}{a_{11}^{(1)}} & 1 & 0 \\
 -\frac{a_{31}^{(1)}}{a_{11}^{(1)}} & 0 & 1 \\\end{bmatrix}

\begin{bmatrix} a_{11}^{(1)} & a_{12}^{(1)} & a_{13}^{(1)} \\
 a_{21}^{(1)} & a_{22}^{(1)} & a_{23}^{(1)} \\
 a_{31}^{(1)} & a_{32}^{(1)} & a_{33}^{(1)} \\\end{bmatrix}

=
\begin{bmatrix} a_{11}^{(2)} & a_{12}^{(2)} & a_{13}^{(2)} \\
 0 & a_{22}^{(2)} & a_{23}^{(2)} \\
 0 & a_{32}^{(2)} & a_{33}^{(2)} \\\end{bmatrix}
\\
{\color{blue} E_{1}}\;\;\;\;\;\;\;\;A^{(1)}\;\;\;\;\;\;\;\;=\;\;\;\;\;\;A^{(1)}
$$

As you can see above, the first column of A looks like an upper triangular matrix. The following steps are exactly the same concept.

- (column2) Remain (2,2) element of A and remove all other elements in the second column of A.

$$
\begin{bmatrix} 1 & 0 & 0 \\
 0 & 1 & 0 \\
 0 & -\frac{a_{32}^{(2)}}{a_{22}^{(2)}} & 1 \\\end{bmatrix}

\begin{bmatrix} a_{11}^{(2)} & a_{12}^{(2)} & a_{13}^{(2)} \\
 0 & a_{22}^{(2)} & a_{23}^{(2)} \\
 0 & a_{32}^{(2)} & a_{33}^{(2)} \\\end{bmatrix}

=
\begin{bmatrix} a_{11}^{(3)} & a_{12}^{(3)} & a_{13}^{(3)} \\
 0 & a_{22}^{(3)} & a_{23}^{(3)} \\
 0 & 0 & a_{33}^{(3)} \\\end{bmatrix}
\\
{\color{blue} E_{2}}\;\;\;\;\;\;\;\;A^{(2)}\;\;\;\;\;\;\;\;=\;\;\;\;\;\;A^{(3)}
$$

(* superscript refers the number of step. Since the value of A keeps changing, this notation is needed to prevent confusion)

- Therefore $B=E_{1}E_{2}$ and it makes A into an upper triangular. From here, all you have to do is simple math to solve the equation instead of inverting.

### Additional Knowledge
- pivot element : diagonal term of upper triangular A is called "pivot element"

### Implementation in R
Now, we should implement this in the real computing.
There is one thing to consider when implementing this method.
In implementation,

$$
\begin{bmatrix} 0 & 1 \\ 1 & 0 \\\end{bmatrix}\;\;\;is actually,\;\;\;
\begin{bmatrix} 1 & 0 \\ 0 & 0 \\\end{bmatrix}
$$

In other words, you should change the order of row ahead(which called 'permutation' _ changing the order) when implementing GE. Therefore send the biggest row to the pivot location before you calculate the matrix E.

The following code is R code, working as Gaussian Elimination.

```
GE <- function(A, b){
  n <- dim(A)[1]
  
  # combine A and b
  Ab <- cbind(A, b)
  
  for(i in 1:(n-1)){
    
    # calculate the order
    ord <- order(abs(Ab[i:n,i]), decreasing = TRUE) + (i - 1)
    
    if (i == 1){
      ord <- ord
    } else {
      k <- i-1
      ord <- c(seq(1:k),ord)
    }
    d <- diag(1, nrow = n, ncol = n)    
    
    # create permutation matrix
    P <- d[ord,]
    
    # permute
    Ab <- P %*% Ab
    
    # create elimination matrix
    d[(i+1):n, i] <- outer(-Ab[(i+1):n, i], Ab[i, i], "/")
    
    # eliminate
    Ab <- d %*% Ab
  }
  
  x <- backsolve(Ab[,1:n], Ab[,n+1], upper.tri = TRUE)
  return(x)
}
```
