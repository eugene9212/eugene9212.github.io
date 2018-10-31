---
layout: post
title:  "Ch1. Matrix Decomposition _ (3.1) Gram Schmidt"
date:   2018-10-30
author: Eugene Lee
categories: "Statistical-Computing"
mathjax: true
---

# Chapter 1. Matrix Decomposition
## 1.3 QR Decomposition

---

### 1.3.1 Introduction of QR decomposition
#### Motivation
Before start this topic, let's briefly introduce Linear Regression for real example.

- linear regression : $y=\beta X + e\;\;\;\;\;e\overset{iid}\sim N_{p} (0,\sigma^2 I_{p})$

- Estimation on $\beta$ with Least Square Method\\
min$(y-\beta x)^{T}(y-\beta x)$\\
Take partial derivative of this equation with respect to $\beta$,\\
$\frac{\partial}{\partial \beta} (y-\beta x)^{T}(y-\beta x)\overset{set}= 0$\\
$\Leftrightarrow x^{T}x\beta=x^{T}y$\\
$\Leftrightarrow \hat{\beta}_{OLS} = (x^{T}x)^{-1}x^{T}y\\

other topics related to this are,\\
(1) What if $(x^{T}x)$ is not invertible? (if $(x^{T}x)$ is linearly dependent)\\
(2) (n < p) high dimensional case\\
and so on\\

But in this topic our main interest is inverse. We focus on much easier way instead of inverting. And this QR decomposition is often used in Linear regression in computing, and it makes solving regression process much easier.

Considering our previous topic, Cholesky Decomposition, we also can solve $X^{T}X\beta = X^{T}y$ with Cholesky method.

1) Compute $X^{T}X$ and $X^{T}y$\\
2) cholesky decomposition on($X^{T}X$) then $LL^{T}$ calculated\\
3) $LL^{T}\beta=X^{T}y$ (set $L^{T}\beta = w$)\\
   $\Leftrightarrow Lw=X^{T}y$\\
   Since L is lower triangular matirx we can simply find w by forward substitution.\\
4) $L^{T}\beta = w$\\
Since $L^{T}$ is upper triangular matrix, back substitution will returns the $\beta$\\
Therefore without inverting $X^{T}X$, we can get estimated $\beta$ with simple method.

- Disadvantages on Cholesky Decomposition

However in real world, this kind of procedure is not widely used. It may seem simply, but it has low accuracy and condition number does not make a big improvement compared to inverting.

Therefore we need a new approach, QR decomposition, and this method is the most standart way to solve regression.

---

#### Basic Concept
Decompose the target matrix X into Q and R parts.\\
$X=QR$\\
Here, Q and R each represents as follows.\\
(1) Q : orthogonal matrix ($Q^{T}Q=I_{p}$)\\
(2) R : upper triangular matrix\\

- Interpretation of Q\\
Linear regression in Linear Algebraic persepctive is a projection of y onto X's column space. Here in QR decomposition, Q is another basis which spans exactly the same space as X's column does. In other words, if the coordinate directions of X is not well organized, like (a), organized htese coordinates into more explicit way, like (b), will make the regression problem  trivial!
<img src="{{ site.baseurl }}/assets/qr.png" height="150">
Therefore, trying to find othogonal basis which spans like X's column space is essential point of this method. Q can be said as coordinate transformation.

- Interpretation of R\\
R will be understood as a constant term.
Just keep
$$
span(X) = span(Q)
$$
in mind. R is just a constant term and finding Q matrix is the point.

There are three methods in QR decomposition, Gram schmidt orthogonalization, Householder transformation, and Givens transformation.

---
### 1.3.2 Gram-Schmidt Orthogonalization
#### Set up
$$
X = Q R
$$

$$
\begin{align}
(\underline{x}_1 \underline{x}_2 \underline{x}_3) &= (\underline{q}_1 \underline{q}_2 \underline{q}_3) \begin{bmatrix} r_{11} & r_{12} & r_{13}\\ 0 & r_{22} & r_{23}\\ 0 & 0 & r_{33}\\\end{bmatrix}\\
&= (r_{11}\underline{q}_1,\;r_{12}\underline{q}_1+r_{22}\underline{q}_2,\;r_{13}\underline{q}_1+r_{23}\underline{q}_2+r_{33}\underline{q}_3)
\end{align} \cdots linear\;combination\;of\;q\\
$$
$$
\underline{x}_i : n \times 1 \text{vector}, \underline{q}_i : n \times 1 \text{vector}
$$

#### Explanaion
$(r_{11}\underline{q}_1,\;r_{12}\underline{q}_1+r_{22}\underline{q}_2,\;r_{13}\underline{q}_1+r_{23}\underline{q}_2+r_{33}\underline{q}_3)$

$
\Rightarrow\underline{x}_1=r_{11}\underline{q}_1\\
\;\;\;\Leftrightarrow \underline{q}_1=\frac{x_1}{\left\lVert x_{1}\right\rVert},r_{11}=\left\lVert x_{1}\right\rVert \\
$

$\Rightarrow\underline{x}_2=r_{12}\underline{q}_1+r_{22}\underline{q}_2$\\
This can be seen as linear regression.(response: $x_{2}$, covariates: $q_{1},q_{2}$)\\
Then coeffecient, $r_{12}=q_{1}^{T}x_{2}$\\
residual:$r_{22}q_{2}=x_{2}-r_{12}q_{1}$\\
$\Rightarrow r_{22}=\left\lVert x_{2}-r_{12}q_{1}\right\rVert,  q_{2}=\frac{x_{2}-r_{12}q_{1}}{r_{22}}$

- detailed explanantion\\
About regression between $x_{2}$ and $q_{1}$, it we regress $x_{2}$ on $q_{1}$ then $r_{12}=q_{1}^{T}x_{2}$ and it can be viewed as $r_{12}=(q_{1}^{T}q_{1})^{-1}q_{1}^{T}x_{2}.$\\
This form is same in ordinary regression $\hat{\beta}$ is eqaul to $(X}^{T}X)^{-1}X^{T}y$)\\
About residual, residual is always orthogonal with covariate. therefore $r_{22}q_{2}$ is always orthogonal to $q_{1}$\\

These ideas can be applied in solving $x_{3}$ as well. \\

$\Rightarrow\underline{x}_3=r_{13}\underline{q}_1+r_{23}\underline{q}_2+r_{33}\underline{q}_3$\\
This can be seen as linear regression. (response: $x_{3}$, covariates: $q_{1},q_{2},q_{3}$)\\
regress $x_{3}$ on $q_{1},q_{2}$.\\
Then coeffecient $r_{13}=q_{1}^{T}x_{3}$, $r_{23}=q_{2}^{T}x_{3}$\\
residual:$r_{33}q_{3}=x_{3}-r_{13}q_{1}-r_{23}q_{2}$\\
$\Rightarrow r_{33}= \left\lVert \text{residual}\right\rVert\\
q_{3}=\frac{residual}{\left\lVert \text{residual}\right\rVert}
$

Extending into p dimension, everything works the same.

#### Meaning
- Using Gram Schmidt, we can move original problem into othonormal coordinate and make canonical form. Therefore every theorem can be developed simply.
- Also, Gram-Schmidt is the most interpretable method to understand QR decomposition.
- Relationship between Cholesky Decomposition\\
$LL^{T}\beta = R^{T}Qy$.  In fact, $L=R^{T}$ \\

- Up to this is regular Gram-Schmidt method. There is also a modified version of Gram-Schmidt, and it has improved accuracy. Therefore most of the computer program utilize modified version of Gram-Schmidt.

#### Algorithm
(1) Initialize\\
$\;\;\;\;r_{11}=\left\lVert x_{1}\right\rVert$, $q_{1}=\frac{x_1}{r_{11}}$\\
(2) Update for the ith iteration (i=2,...,p)\\
$\;\;\;\;r_{ij}=q_{i}^{T}x_{j}$    $j=1,\cdots ,i-1$\\
$\;\;\;\;m_{i}=x_{i}-\sum_{j=1}^{i-1} r_{ji}q_{i}\cdots residual$\\
$\;\;\;\;r_{ii}=\left\lVert m_{i} \right\rVert$, $q_{i}=\frac{m_i}{\left\lVert m_{i} \right\rVert}$
    
#### Code

























