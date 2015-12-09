---
layout: post
title: "Linear Regression : Frequentist and Bayesian"
excerpt: "Post about linear regression methods in frequentist and bayesian style."
tags: [machine learning, data science, mathematics, statistics]
comments: true
image:
  feature: sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---
We often hear there are two schools of thought in statistics - Frequentist and Bayesian. A the very fundamental level the difference in these two approaches stems from the way they interpret probability. For a frequentist, probability is defined in terms of limiting frequency of occurrence of an event while a bayesian statistician defines probability as the degree of disbelief on the occurrence of an event. This post in not about the philosophical aspects of the debate. Rather we will study an example from frequentist and bayesian methods. The example we will consider is the linear regression model. 
<br><br>
**Setup:** 
<br>
Let the data be \\( D = \( x\_{i} , y\_{i} \)\_{1 \leq i \leq N} \\) where each \\( x\_{i}  \in \mathbb{R}^n \\) and \\( y\_{i} \in \mathbb{R} \\).
The linear regression model predicts the values of \\( y\_{i} \\)'s as linear combinations of the features \\( x\_{i} \\)'s

$$ y\_{i} = w\_{0} + \sum\_{j} w\_{j} x\_{ij} = w\_{0} +w^T x\_{i} $$
