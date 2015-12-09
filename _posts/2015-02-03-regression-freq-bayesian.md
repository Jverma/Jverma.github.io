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

Adding 1 to the vectors \\(( x\_{i} \\)) to redefine \\(( x\_i = (1,x\_i) \\)) and combining \\(( w\_0 \\)) and \\(( w\_i \\))'s into a single vector \\(( w = (w\_0, w\_1, \ldots, w\_n) \\)), this can be written as 

$$ y\_i = w^T x\_i $$

The idea is to estimate the values of parameters \\(( \hat{w} \\)) from the training data and predict the \\(( y \\))-value for a new observation \\(( \tilde{x} \\)) as 

$$ \tilde{y} = \hat{w}^T \tilde{x} $$
<br><br>
We will consider *Maximum likelihood estimation* (Frequentist), *Maximum a Posteriori* (semi-bayesian) and *Bayesian regression models*. 
<br><br>
**tl;dr**
<br>
MLE chooses the parameters which maximize the likelihood of data given that parameter, MAP chooses parameters which maximize the posterior probability  of that parameter in the light of observed data and Bayesian inference computes the posterior probability distribution for the parameters given data.
<br><br>
**Maximum Likelihood Estimation:**
The observed values of \\(( y_i \\))  is assumed to have Gaussian noise error i.e. 

$$ y\_i = w^T x\_i + \epsilon $$

where \\(( \epsilon \sim N(0,\sigma^2) \\)).
<br>
The Likelihood in this case is given by
<br>
$$ \mathcal{L}(D | w, \sigma) = (2 \pi \sigma^2)^{-n/2} \prod\_{i=1}^{n} exp \left[ \frac{-(y\_i - \hat{y}\_i)^2}{2\sigma^2} \right] $$
<br>
then the log-likelihood is given by
<br>
$$ \ln(\mathcal{L}) = - \frac{n}{2} \ln(2\pi \sigma^2) -  \frac{1}{2\sigma^2} \sum\_i (y\_i - w^T x\_i)^2 $$
<br>
Now MLE states that the estimated value of \\(( w \\)) is given by
<br>
$$w\_{MLE} = argmax\_w \ln(\mathcal{L}(D | w, \sigma)) $$
<br>
which in this case reduces to
<br>
$$ w\_{MLE} = argmin\_w \sum\_i  (y\_i - w^T x\_i)^2 $$
<br><br>
This is why the linear regression model is often known as *least square method*. 
Now we differentiate with respect to \\(( w \\)) and equate the derivative to zero to get the estimate of \\(( w \\)). This can be more clearly expressed in terms of linear algebraic quantities.
 <br><br>
If \\(( X = (x\_1, x\_2, \ldots, x\_N)^T \\)), \\(( Y = (y\_1, y\_2, \ldots, y\_N)^T \\)) and  \\(( \theta = (w\_0, w\_1, w\_2, \ldots, w\_N)^T \\)), then one can check that the MLE for \\(( \theta \\)) is 
<br>
$$ \hat{\theta} = (X^T X)^{-1} X^T Y $$
<br>
Similarly  \\(( \sigma^2 \\)) can be estimated by differentiating the MLE with respect to  \\(( \sigma^2 \\)) and equation the derivative to zero. 
<br><br>
**Maximum a Posteriori estimation:**
<br>
 For MAP, we assume a Gaussian prior on \\(( w \\)) i.e 
$$ w \sim N(0, \lambda^{-1} I)$$
<br>
$$ P(w) = (\frac{\lambda}{2 \pi})^{n/2} exp \left[ -\frac{\lambda}{2} w^T w \right] $$
<br>
Then the posterior probability after we observe the training data is computed by Bayes rule as 

$$ P(w|D) = \frac{P(w) P(D|w)}{P(D)}$$
<br>
as with MLE, we will maximize the log-posterior probability and then 
<br>
$$ w\_{MAP} = argmax\_w \ln(P(w|d)) $$
<br>
which reduces to

$$ w\_{MAP} = argmin\_w \sum\_i (y\_i - w^T x\_i)^2 + \frac{\lambda}{2} w^T w $$
<br>
Thus, the MAP estimation can be thought of as regression with regularization. 
The MAP estimate in this case is 
<br>
$$ w\_{MAP} = (\lambda I + X^T X)^{-1} X^T y $$

<br><br>
**Bayesian Regression:**
<br>
 In Bayesian regression, the Bayesian philosophy is applied. Both MLE and MAP are point estimates but in Bayesian regression, we look for predictive probability. Here we make predictions by integrating over the posterior distribution of the model parameters \\(( w \\)). 
If \\(( \tilde{x} \\)) is a new point, we compute the probability of \\(( \tilde{y} \\)), the y-value corresponding to this x is given by
<br>
$$ P(\tilde{y} | \tilde{x}, D, \sigma^2, \lambda) = \int P(\tilde{y} | w, \tilde{x}, \sigma^2 ) P(w|D, \sigma^2 , \lambda) dw $$
<br>
These integration are often very hard to to do analytically and rely on sophisticated MCMC methods. 
 <br><br>
In full Bayesian regression, we assume a prior on \\(( \sigma^2 \\)) in addition to prior on \\(( w \\)).
