---
layout: post
title:  "Matrix Decomposition _ (3) QR Decomposition"
date:   2018-10-26
author: Eugene Lee
categories: "Statistical-Computing"
mathjax: true
---

# Matrix Decomposition
## Chapter 3. QR Decomposition
### Motivation
Before start this topic, let's briefly introduce Linear Regression for real example.

- linear regression : $y=\beta X + e\;\;\;\;\;e\overset{iid}\sim N_{p} (0,\sigma^2 I_{p})$

- Estimation on $\beta$ with Least Square Method
$\operatorname*{min}_\beta (y-\beta x)^{T}(y-\beta x)$
as we take partial derivative of this equation with respect to $\beta$,
$\frac{\partial}{\partial \beta} (y-\beta x)^{T}(y-\beta x)\overset{set}= 0$
$\Leftrightarrow x^{T}x\beta=x^{T}y\;\;\;\;\right arrow$"normal equation"
$\Leftrightarrow \hat{\beta}_{OLS} = (x^{T}x)^{-1}x^{T}y

other topics related to this are,
(1) What if $(x^{T}x)$ is not invertible?(if $(x^{T}x)$ is linearly dependent)
(2)(n < p) high dimensional case
and so on.

But in this topic our main interest is inverse. We focus on much easier way instead of inverting. And this QR decomposition is often used in Linear regression in computing, and it makes solving regression process much easier.

Considering our previous topic, CHolesky Decomposition, we also can solve $X^{T}X\beta = X{T}y$ with Cholesky method.
\romannumeral2)

