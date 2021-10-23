---
title: "Advanced Econometrics - Machine Learning - Multiclass Logistic Regression, K-means clustering and Principal Component Analysis"
categories:
  - Projects
  - Matlab
tags:
  - Matlab
  - Machine Learning
  - Clustering
  - Classification
  - Unsupervised Learning
excerpt: "Advanced Econometrics Homework on Multiclass Logistic Regression, K-means clustering and Principal Component Analysis" 
header:
  overlay_image: /assets/images/posts/kmeans/iris_flower.png
  overlay_filter: 0.5 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**w3resources**](https://www.w3resource.com/machine-learning/scikit-learn/iris/index.php)"
---

## Table of Contents
1. [Introduction](#introduction)
2. [Data](#data)
3. [Model - Multiclass Logistic Regression and K-means Clustering](#model)
4. [Model - Principal Component Analysis](#model)
5. [Code](#code)
6. [Take Aways](#takeaways)

# Introduction

As an exercise in multiclass logistic regression, k-means clustering and principal component analysis, I generated a prediction model on the famous IRIS data set, to classify the flowers based on sepal and petal characteristics into one of three categories. 

# Data

The Iris flower dataset is a famous data set used in machine learning classes. More information on the data can be found on the [wikipedia](https://en.wikipedia.org/wiki/Iris_flower_data_set) page.

The data contains 150 rows with the petal length and width, and sepal length and width of each individual flower. It is split 50/50/50 between the three types of Irises: Setosa, Versicolor and Virginica. The aim of the exercise is to create a model that can accurately cluster the flowers into the three categories.

# Model - Multiclass Logistic Regression and K-means Clustering

I first tested a linear boundary between the three classifications, after conducting a multiclass one-vs-all logistic regression on the data.

![linearboundary.jpg](/assets/images/posts/kmeans/linearboundary.jpg)

Since we know the true classification, we can test our model accuracy using a confusion matrix.

![linearboundarycmatrix.jpg](/assets/images/posts/kmeans/linearboundarycmatrix.jpg)

Per the confusion matrix, our classification is fairly accurate, with prediction accuracy for the model at 92%. 

The next model tested was through using k-means clustering to group the data points. I derived optimum k = 3 through the elbow method. Post clustering, the results are as below:

![kmeans1.jpg](/assets/images/posts/kmeans/kmeans1.jpg)

![kmeans1cmatrix.jpg](/assets/images/posts/kmeans/kmeans1cmatrix.jpg)

Model accuracy is slightly lower at 89.3%. The chief areas of inaccuracy are between classifications for Versicolor and Virginica.

# Model - Principal Component Analysis

I also conducted a principal component analysis on the dataset. The respective variances explained by each component are below:

Variance explained by principal component 1 = 80.59%
Variance explained by principal component 2 = 14.92%
Variance explained by principal component 3 = 2.91%
Variance explained by principal component 4 = 1.58%

Graphically, post PCA component reduction, we see the below representations:

For 1 component:
![pca1d.jpg](/assets/images/posts/kmeans/pca1d.jpg)

For 2 components:
![pca2d.jpg](/assets/images/posts/kmeans/pca2d.jpg)

For 3 components:
![pca3d.jpg](/assets/images/posts/kmeans/pca3d.jpg)

I also conducted k-means clustering on the reduced data, for 1, 2 and 3 principal components. The clusters are below:

For 1 component:
![kmeans1pca.jpg](/assets/images/posts/kmeans/kmeans1pca.jpg)

For 2 components:
![kmeans2pca.jpg](/assets/images/posts/kmeans/kmeans2pca.jpg)

For 3 components:
![kmeans3pca.jpg](/assets/images/posts/kmeans/kmeans3pca.jpg)

Given the levels of variance explained by each component, I expect the k-means clustering for 2 components to perform much better than the clustering for 1 component. I also expect the clustering for 3 components to be around equal to that of 2 components. 

This was indeed the case in the model. Prediction accuracy was only 84% for 1 component, rising to 88.67% for 2 components, and with only a slight rise to 89.3% for 3 components. There was no change between the model with 3 components vs. the model with all components. 

# Code

The full self written MATLAB code is available on the github page for the [project](https://github.com/Jwangjy/kmeans).

# Take Aways

1. PCA is a potential way to reduce the size of datasets greatly, as well as remove the impact of multicollinearity within the dataset. However, it can become expensive to perform for large datasets.

2. K-means clustering results are dependent on the rng seed, as different starting cluster locations could lead to slightly different answers. 

3. Model accuracy is highly dependent on the most important principal components by singular values. It is important to check these prior to reducing the model.