---
layout: post
title: "Manifold learning: Introduction"
excerpt: "First post in the series on manifold learning techniques."
tags: [mathematics, machine learning, data mining, data science, differential geometry]
comments: true
<!-- image:
  feature: sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/ -->
---
<strong>Manifold learning</strong> is one of the most popular approaches to dimensional reduction. The idea is that the data which seems to be high dimensional e.g. with thousands of features, actually lies on a low dimensional manifold embedded in the ambient (high dimensional) Euclidean space. The goal of the manifold learning techniques is to 'learn' the low dimensional manifold.
<br><br>
The manifold learning methods provide a way to extract the underlying parameters of the data, which are much lesser in number that the original dimension, and can explain the intricacies of the data. The assumption is that the data lies along a low-dimensional manifold which describes the underlying parameters and the ambient high-dimensional space is the <em>feature space</em>.
<br><br>
The main applications of dimensional reduction are as follows :

- <strong>Feature Selection:</strong> In cases when we have a very large number of features, a lot of them are irrelevant or misleading. This can lead to overfitting (variance) or underfitting (bias) in our model. Also too many features make the algorithm very slow and it may take a lot of time to converge. Dimensional reduction techniques are used to vastly reduce the number of features.
- <strong>Data Visualization:</strong> If the data is very high dimensional, we cannot get an intuitive idea of what the data looks like. Dimensional reduction to two or three meaningful features provides a great way to visualize the data.
- <strong>Discovery of latent pattern in data:</strong> Dimensional reduction can also unveil some hidden structure in the data. For example, suppose we are building a search engine for text articles. The user submits a query and the system should return a list of documents most relevant to the query. A classical approach is the so called vector space model, where we represent each document as a vector based on the words contained in the document. Such a representation is very high dimensional. If the user asks for results related to 'dog', the documents containing word 'dog' will be retrieved, but the documents which are related to `dog', and use synonyms of 'dog' e.g. 'canine' etc., but don't contain 'dog' in high enough frequency will not be returned. A strategy to resolve this problem is to use dimensionality reduction to compute a more realistic representation of the documents, where 'dog' and 'canine' documents are very close to each other. This method is called <em>Latent Semantic Analysis</em>, and can be interpreted as a representation of documents in terms of topics as opposed to words. Another such example is <em>population stratification</em> in computation genomics, when the population of interest includes subgroups of individuals that are on average more related to each other than the other members of the population.

<br><br>
Before we delve into manifold learning methods, let's review the simplest dimensional reduction method, called PCA which is a linear method and later we will generalize manifold learning as non-linear extension of PCA.
<br><br>
<strong>Principal Component Analysis:  </strong>
<br>
PCA is the simplest and most popular dimensional reduction method. Consider a data set containing n points, \\( D = \{ x\_1,x\_2,...,x\_n \} \\) where each \\( x\_i \in \mathbb{R}^m \\) is a *feature vector*. PCA attempts to find the directions along which the data has maximum variance. We then project the data vectors onto these directions (lesser in number than original dimensions) to obtain a low-dimensional representation of the data. PCA assumes that the data points lie on or near a <em>linear subspace</em> of the feature space (\\( \mathbb{R}^m \\) ). This is a crucial assumption, one we will abandon later for more generality. If a sample of points drawn from 3-dimensional Euclidean space actually lie on a 2-dimensional plane, then projection on first two principle components will return the plane on which data lies.
<br><br>
More accurately, PCA solves the following optimization problem :
<br>
<em>Given a matrix whose rows are m−dimensioanl data points -
<br><br>
 $$ X = (x\_1,x\_2,\ldots,x\_n)^T \in \mathbb{R}^{n \times m}  $$
<br><br>
Find \\( Y \subset \mathbb{R}^m \\) such that the data along this subspace has maximum variance.</em>
<br>
A solution of such optimization problem is obtained to computing the <em>Singular Value Decomposition</em> (SVD) of X.
<br><br>
PCA has been very successful in a lot of dimensional reduction tasks. For example latent semantic analysis mentioned earlier uses PCA, population structure in the genetic data from different geographical locations can be inferred using PCA etc.

<br><br>
Despite it’s popularity, PCA has some obvious shortcomings, most notably is the assumption that data lie on a linear subspace. Consider the data that lie along a curled plane, e.g. a <em>swiss roll</em> embedded in 3-dimensional Euclidean space.
<figure>
<img class="alignnone  wp-image-1045" src="https://januverma.files.wordpress.com/2015/12/file-dec-06-6-32-46-pm.png" alt="File Dec 06, 6 32 46 PM.png" width="632" height="473" />
</figure>
<br>
It is clearly 2-dimensional embedded in 3-dimensional Euclidean, but is not a linear subspace. PCA would not be able to correctly decipher the underlying structure. The swiss roll is actually a 2-dimensional submanifold of the Euclidean space. This and many such example ask for a more general framework for dimensional reduction where we disband the linear subspace requirement (as in PCA) to capture the non-linearities in the dataset.
<br><br>
Manifold learning can be thought of a non-linear generalization of PCA, where the data in no longer assumed to lie on a <em>linear subspace</em>. Instead we assume that the data lie on a low-dimensional manifold embedded in the feature space.
<br><br>
Next we will develop the mathematical machinery from differential topology and geometry which is required to understand the notion of manifolds.
<br><br>
<strong>Basics of Geometry:  </strong>
<br>
<strong>Definition:</strong> Let \\( U \subset \mathbb{R}^{n} \\) and \\( V \subset \mathbb{R}^{k} \\) be open sets, and \\( f : U \rightarrow V \\) be a function. Then \\( f \\) is <strong>smooth</strong> if all of the partial derivatives of \\( f \\) are continous. i.e. for any \\( m \\)
\\( \frac{\partial^{m}f}{\partial x\_i \ldots \partial x\_m} \hspace{5mm} \text{is smooth}. \\)
<br><br>
We have not rigorously defined open sets in a <em>topological space</em> (e.g. \\( \mathbb{R}^{n} \\), intuitively they are extensions of open intervals on real line. And we don't need the notion of continuity of a function in its full glory. In this article, we are restricting to sets in Euclidean spaces.
<br><br>
Smoothness of a function can be extended to arbitrary sets (not necessarily open) as follows.
<br>
<strong>Definition:</strong> Let \\( X \subset \mathbb{R}^{n} \\) and \\( Y \subset \mathbb{R}^{k} \\) be arbitrary subsets. A function \\( f : X \rightarrow Y \\) is <strong>smooth</strong> if for each \\( x \in X \\), there exists an open neighborhood \\( U \subset \mathbb{R}^n \\) and a smooth function \\( F: U \rightarrow \mathbb{R}^k \\) that coincides with \\( f \\) on \\( U \cap X \\).
<br><br>
<strong>Definition:</strong> A function
\\( f : X \rightarrow Y \\) is a  <strong>homeomorphism</strong> if it is a <em>bijection</em> of sets with both \\( f \\) and \\( f^{-1} \\) continous. A homeomorphism is called a<strong> diffeomorphism</strong> if both \\( f \\) and
\\( f^{-1} \\) are smooth.
<br><br>
Equipped with this terminology, we are now ready to define a manifold. <em>A manifold is a topological space which is locally diffeomorphic to some Euclidean space.</em> To be more precise,
<br><br>
<strong>Definition:</strong> Let \\( M \subset \mathbb{R}^{n} \\), then \\( M \\) is a <strong>smooth manifold</strong> of <strong>dimension d</strong> if for each \\( x \in M \\), there exists an open neighborhood \\( U \\) containing \\( x \\) and a diffeomorphism \\( f: U \rightarrow V \\) where \\( V \subset \mathbb{R}^{d} \\) plus some compatibility conditions.
<br>
These open neighborhoods are called the <strong>coordinate patches</strong> and the diffeomorphisms are called <strong>coordinate charts</strong>.

<br><br>
<strong>Some abstract non-sense:</strong>
<br>
Based on previous discussions, we need to move to the next general <em>category</em> of <em>spaces</em> (The notion of space is intentionally left ambiguous). To extract linear subspaces, e.g. in PCA, we work in the category of vector spaces over the <em>field</em> of real numbers. These spaces are <em>globally flat</em> (no <em>curvature</em>). In fact, all the datasets we would encounter will be subsets of Euclidean spaces  \\( \mathbb{R}^n \\)). The next step in abstraction is to lift to the category of  <em>locally flat spaces </em>i.e. the spaces which look like vector spaces locally. Mathematically this is the category of smooth real manifolds <em>embedded</em> in \\( \mathbb{R}^n \\) and it includes vector spaces over reals as a <em>subcategory</em>. In the same spirit, we look for <em>submanifold embeddings</em> of original data manifold.
<br><br>
Every real vector space is a real manifold.
For more curious, there is a more general category of spaces called <em>topological spaces </em>and there is a very active of research called <strong><em>topological data analysis</em></strong>, where the data points are assumed to be sampled from a topological space. This is the most general category of mathematical spaces that the author is aware of. Evidently TDA has shown great progress in unraveling hidden structure in variety of datasets.
Schematically,
<br><br>
$$ Vect\_{\mathbb{R}} \subset Manifolds\_{\mathbb{R}} \subset Top\_{\mathbb{R}} $$

<br><br>
**PCA as manifold learning:**
<br>
Every d-dimensional vector space \\( V \\) over the field of reals is isomorphic to the Euclidean space \\( \mathbb{R}^{d} \\). And is thus trivially a smooth manifold with a single *coordinate chart*
<br>
$$ Identity : V \rightarrow \mathbb{R}^d $$
<br>
It can be seen that the linear subspaces of \\( V \\) are the submanifolds. If the data actually lies along a linear subspace of the feature space, the manifold learning methods will recover the linear subspace as the sought after submanifold embedding. The results will be very similar to what you will obtain by applying PCA. Thus PCA is the simplest type of manifold learning.
<br><br>
In future posts, we will talk about various manifold learning methods. 

