---
layout: post
title:  "ABC"
categories: [ General, model evaluation ]
author: ghost
image: assets/images/2.jpeg
---


Statistical Decision Theory is the "Schroedinger Equation" of classification answers questions. 

*What does the best conceivable classifier look like? - Bayes classifier*. 

*How good is this classifier - Bayes risk*. 

Define the **Loss function** \\( L(y,\hat{y}) \\) where \\( y \\) is the true and \\( \hat{y} \\) predicted class, e.g. symmetric/asymmetric *0-1-loss*. Aim of classification is to minimize the expected loss="risk"  \\( R \\) of classifier \\( f \\):

$$
    \small R(f) := \mathbb{E}_x\mathbb{E}_yL(y,f(x)) = \int_x\mathbb{E}_yL(y,f(x))p(x)dx = \sum_{y\in Y}\sum_{z\in Y}L(y,z)I[f(x)=z]p(y|x)
$$

where \\( \mathbb{E} \\) is taken w.r.t. true distributions. For symmetric *0-1-loss*, i.e. loss of 1 for wrong predictions, one has:

$$
    \small  \mathbb{E}_y L(y,f(x)) = \sum_{y\in Y /f(x)} 1 \cdot p(y|x) = 1-p(f(x)|x)
$$

since 

$$
    \small  \sum_{y \in Y}p(y|x) = 1
$$

with \\( y \in Y / f(x) \\) is \\( y \\) except for the predicted class and \\( p(y \vert x) \\) is the true posterior which one does not have straightfowardly. One can interpret the result as: the posterior probability of predicted class must be large to minimize the expected risk, i.e. MAP. For 0-1-loss function the ideal classifier is therefore:

$$
    \small  f(x) = \underset{z \in Y}{\operatorname{argmax}} p(z|x)
$$

i.e. Bayes classifier. The actual quality of the classification however largely depend on the data overlap in the feature space.
