---
layout: post
title: "Gibbs Sampling"
excerpt: "Post about gibbs sampling method."
tags: [machine learning, data science, mathematics, statistics]
comments: true
image:
  feature: sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---
[Gibbs sampling](http://en.wikipedia.org/wiki/Gibbs_sampling) is a [Markov chain Monte Carlo](http://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo) method for sampling from a multivariate probability distribution. It has many applications in inference, [Bayesian networks](http://en.wikipedia.org/wiki/Bayesian_network) and machine learning. I am a bioinformatician, who needs to sample from joint distributions all the time. What motivated me to write this post is that currently I'm building a Bayesian network for my work, where I need to draw samples from the network. I'll talk about the use of Gibbs sampling in BayesNets in another post.

Let's start with a simple sampling problem. Suppose we have a single variable \\( X \\) which takes two values:
<br><br>
\\( P(X =0)=0.5 \\) and \\( P(X =1)=0.5 \\)
<br><br>
This is an example of [binomial distribution](http://en.wikipedia.org/wiki/Binomial_distribution). How can get a sample from this distribution?
Simply, flip a coin. If it's head, \\( X=1 \\), else \\( X=0 \\). The following python code will generate samples from this distribution.

{% highlight python %}
import random

def sampler():
	r = random.random()
	if (r < 0.5):
		X = 0
	else:
		X = 1
	return X
{% endhighlight %}
<br>
What if you have a [multinomial distribution](http://en.wikipedia.org/wiki/Multinomial_distribution) ? Say you want to model the roll of a dice:
<br>
$$ P(X = i) = 1/6 ,  i \in \{ 1,\ldots ,6 \} $$
<br>
Divide the interval [0, 1] into 6 equal parts and select the value of \\( X \\) based on the interval in which a pseudo-random number falls.
{% highlight python %}
def sampler():
    r = random.random()
    if (r < float(1)/6):
        X = 1
    elif (float(1)/6 <= r < float(2)/6):
        X = 2
    elif (float(2)/6 <= r < float(3)/6):
        X = 3
    elif (float(3)/6 <= r < float(4)/6):
        X = 4
    elif (float(5)/6 <= r < float(5)/6):
        X = 5
    else:
        X = 6
	return X
{% endhighlight %}
<br>
Suppose we have a multivariate distribution -
<br>
$$ P(X\_1,X\_2, \dots,X\_n) $$
<br>
If the variables are independent of each other (i.e [independence assumption](http://en.wikipedia.org/wiki/Independence_(probability_theory)), then we have
<br>
$$ P(X\_1,X\_2, \ldots,X\_n) = \prod\_{i=1}^{n} P(X\_i) $$
<br>
which will be easy to sample. Draw samples from each of the \\( P(X\_i) \\) individually and then multiply to get a sample of the joint distribution.
<br>
People who work with machine learning and data mining would recognize this assumption to be true for [Naive Bayes algorithm](http://en.wikipedia.org/wiki/Naive_Bayes_classifier).
<br><br>
But if the independent assumption doesn't hold, the sampling problem is much harder. Gibbs sampling gives a procedure to sample from a joint probability distribution in terms of conditionals. This efficient method works under one condition:
<br>
*It is easy to sample from conditional distributions* 
$$ P(X\_i | X\_1,X\_2, \ldots, X\_{i-1},X\_{i+1}, \ldots X\_n) $$ 
*for all* \\( X\_i \\)
<br><br>
**Algorithm:**
<br><br>

￼1. Specify an initial value \\( x^{(0)} = (x\_1 \ldots ,x\_n) \\)

2. Iterate for \\( j = 1,2,3,\ldots \\)

- Pick an index \\( i \\) for \\( 1 \leq i \leq n \\) uniformly at random.
-  Sample \\( x_i \\) from \\( P(X\_{i} | x^{(j-1)}\_{(-i)}) \\)
- \\( x^{(j)} = (x^{(j-1)}\_{(-i)}, x\_{i}) \\)

<br><br>
The above sampling methodology generates a sequence of samples \\( x^{(0)},x^{(1)},\ldots \\). 
<p>
Intuitively, the relation between joint and conditional distributions play an important role in this approximation of joint distribution by conditionals.  
<br>
$$ P(X\_i | X\_1,X\_2, \ldots, X\_{i-1},X\_{i+1}, \ldots X\_n) = \frac{P(X\_1,X\_2, \ldots,X\_n)}{P(X\_1,X\_2, \ldots, X\_{i-1},X\_{i+1}, \ldots X\_n)} $$
<br>
More theoretically, it can be shown that this sequence forms a [Markov Chain](http://en.wikipedia.org/wiki/Markov_chain) over all the possible states. The *stationary state* of this Markov chain is the sought after joint distribution \\( P(X\_1,X\_2, \ldots,X\_n) \\). I'll not go into the theory in this post, a curious soul can refer to other resources.
<br><br> 
Gibbs sampling is a special case of the [Metropolis-Hastings algorithm](http://en.wikipedia.org/wiki/Metropolis%E2%80%93Hastings_algorithm).
<br><br>
A nice implementation of the basic idea of Gibbs sampling can be found on [Darren Wilkinsen's blog](http://darrenjw.wordpress.com/2011/07/16/gibbs-sampler-in-various-languages-revisited/). 