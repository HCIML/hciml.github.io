---
layout: post
title:  "Basic: Bayesian Model Selection"
categories: [ General, model evaluation ]
author: ghost
featured: true
image: assets/images/9.jpg
---

The goal of this short post is to discuss on Bayesian Model Selection being an effective theoretical tool of assessing relative performance  of a fixed number of models at hand. Having a fixed set of models the **model posterior probability** of a model given some data is:

$$ \small p(M_i|\mathcal{D}) = \frac{p(\mathcal{D}|M_i)p(M_i)}{p(\mathcal{D})} $$ 

The normalization constant is evaluated to:

$$ \small p(\mathcal{D}) = \sum^m_{i=1} p(\mathcal{D}|M_i)p(M_i) $$

The performance of a model can be accessed by the **relative probability of the model** of interest:

$$ \small \frac{p(M_i|\mathcal{D})}{p(M_j|\mathcal{D})} = \frac{p(\mathcal{D}|M_i)}{p(\mathcal{D}|M_j)} \frac{p(M_i)}{p(M_j)} $$

The **model likelihood** for continuous model parameters is given as:

$$ \small p(\mathcal{D}|M_i) = \int p(\mathcal{D}|\theta_i,M_i)p(\theta_i|M_i)d\theta_i $$

For \\( N \\) i.i.d. data points the model likelihood takes the form:

$$ \small p(\mathcal{D}|M_i) = \int p(\theta_i|M_i)\prod^N_{n=1}p(x^n|\theta_i,M_i)d\theta_i $$

and the **unnormalized log-posterior** is:

$$ \small \log p(\mathcal{D}|M_i)p(M_i) = \log p(\boldsymbol{\theta}|M) + \sum^N_{n=1}\log p(x^n|\boldsymbol{\theta},M) $$

## Laplace method
For computing the model likelihood the integral is usually intractable and has to be approximated. The Laplace method to approximate the model log-likelihood for a Gaussian-like posterior is to find \\( \boldsymbol{\theta}^* \\) via MAP and to fit a Gaussian to this point in the parameter space based on the local curvature:

$$ \small
    \boldsymbol{\theta}^* = \underset{\boldsymbol{\theta}}{\operatorname{argmax}} \mbox{ }p (\mathcal{D}|\boldsymbol{\theta},M)p(\boldsymbol{\theta}|M)
$$

$$ \small
    \log p(\mathcal{D}|M) \approx \log p(\mathcal{D}|\boldsymbol{\theta}^*,M)+\log p(\boldsymbol{\theta}^*|M)+\frac{1}{2}\log \det (2\pi \textbf{H}^{-1})
$$

Hereby \\( \textbf{H} \\) is the Hessian of the negative log posterior evaluated at \\( \boldsymbol{\theta}^* \\) .

## BIC Approximation
An even simpler method is to crudely approximate the Hessian \\( \textbf{H} \\) by taking \\( \boldsymbol{H}\approx N\boldsymbol{I}_k \\) where  \\( K=\dim(\boldsymbol{\theta}) \\) is the number of model parameters. With this Hessian the Laplace approximation is:

$$ \small
    \log p(\mathcal{D}|M) \approx \log p(\mathcal{D}|\boldsymbol{D}|\boldsymbol{\theta}^*,M)+\log p(\boldsymbol{\theta}^*|M)+\frac{K}{2}(\log 2\pi -\log N)
$$

One can further approximate the prior and take: 

$$ \small p(\boldsymbol{\theta}|M) = \mathcal{N}(\boldsymbol{\theta}|\boldsymbol{0},\boldsymbol{I}), $$

which penalizes the length of the parameter vector and is favorizing a simple model. With that the log-likelihood is:

$$ \small
    \log p(\boldsymbol{D}|M) \approx \log p(\mathcal{D}|\boldsymbol{\theta}^*,M)-\frac{1}{2}(\boldsymbol{\theta}^*)^T\boldsymbol{\theta}^*-\frac{K}{2}\log N
$$

By ignoring the penalty term for large \\( N \\) one arrives at the definition of the **Bayes Information Criterion (BIC)**:

$$ \small BIC = K\log N -2\log p(\mathcal{D}|\boldsymbol{\theta}^*,M) $$

BIC is closely related to the **Akaike Information Criterion (AIC)** which is:

$$ \small AIC = 2K-2\log p(\mathcal{D}|\boldsymbol{\theta}^*,M) $$
