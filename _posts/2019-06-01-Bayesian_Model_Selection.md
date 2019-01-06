---
layout: post
title:  "Bayesian Model Selection"
author: kiryl
categories: [ General, model evaluation ]
image: assets/images/9.jpg
---
For a fixed set of models the model posterior probability of a model given some data is:
\[
    p(M_i|\mathcal{D}) = \frac{p(\mathcal{D}|M_i)p(M_i)}{p(\mathcal{D})}
\]
The normalization is:
\[
    p(\mathcal{D}) = \sum^m_{i=1} p(\mathcal{D}|M_i)p(M_i)
\]
The relative probability of a model is:
\[
    \frac{p(M_i|\mathcal{D})}{p(M_j|\mathcal{D})} = \frac{p(\mathcal{D}|M_i)}{p(\mathcal{D}|M_j)} \frac{p(M_i)}{p(M_j)}
\]
The model likelihood for continuous model parameters is given as:
\[
    p(\mathcal{D}|M_i) = \int p(\mathcal{D}|\theta_i,M_i)p(\theta_i|M_i)d\theta_i
\]
For $N$ i.i.d. data points the likelihood takes the form:
\[
    p(\mathcal{D}|M_i) = \int p(\theta_i|M_i)\prod^N_{n=1}p(x^n|\theta_i,M_i)d\theta_i
\]
and the unnormalized log-posterior is:
\[
    \log p(\mathcal{D}|M_i)p(M_i) = \log p(\boldsymbol{\theta}|M) + \sum^N_{n=1}\log p(x^n|\boldsymbol{\theta},M)
\]

