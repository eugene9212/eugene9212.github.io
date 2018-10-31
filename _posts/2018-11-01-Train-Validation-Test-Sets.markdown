---
layout: post
title:  "Lecture2. Image Classification Pipeline"
date:   2018-11-01
author: Eugene Lee
categories: "CS231n"
mathjax: true
---

# Lecture2. Image Classification Pipeline
## Setting Hyperparameters
### Train, Validation, and Test Sets
In choosing hyper parameter, three data sets are required: train, validation, and test sets. Therefore before you analyze data, you shoul partition the data into three parts in advance.

(1) Train Set : With train set, you should train algorithm with many different choices of hyperparameters on the training set.

(2) Validation Set : Evaluate on validation set and pick a set of hyper parameter with the best performance on the validation set.

(3) Test Set : Take the best performed hyper parameter set and run it <strong>once</strong> on the test set. Returned value of accuaracy of the model is registered on your paper/report. This number can be understood as how your model is doing on unseen data.

#### Additional comments
Another difference between train set and validation set is related with accessing to true label. \\
Train set can access directly to true label, since it has to build up a model. Validation set does not have a direct access. It can approach to the true label when you evaluate the performance on validation set. Test

---

### Cross Validation
Instead of three partition, cross-validation can be another choice of selecting hyper paramters.

Cross validation method splits data into folds and try each fold as validation and average the accuracy/performance of each folds. For example, 5-fold cross validation method has, 5 folds for train and validate, and test set. one fold in  total 5 folds do validation role in turn. You can split the fold as many as you can, but usually 10 fold is maximum number of fold in general.

This method is useful with small datasets, but not common in deep learning.

#### Why other data partition cases are bad
(1) Using only one data partition \\
It will build up a model which works perfectly on the specific data, train set.

(2) Train and Test set \\
You can construct a model with train set, and choose an hyper parameter with test set. In this case, you do not know this set of hyper parameters actually works well on new data set.


## Linear Classification
Understanding basic concept of linea classification will enhance your knowledge on Neural Network and CNN.

### Parametric Approach
Linear classifier is one of the simplest examples of parametric model.

input : (32 x 32 x 3) image data\\
model : f(x,W) , x refers to input data and W is parameters/weights\\
output : class scores\\

In the above example, W is a set of parameters or weights and it contains the information on train data. Considering KNN, it has no weights and it only use the whole train set.















