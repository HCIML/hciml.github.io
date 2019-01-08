---
layout: post
title:  "Bayesian Model Selection"
categories: [ General, model evaluation ]
author: ghost
image: assets/images/9.jpg
---
For a fixed set of models the model posterior probability of a model given some data is:

$$
    p(M_i|\mathcal{D}) = \frac{p(\mathcal{D}|M_i)p(M_i)}{p(\mathcal{D})}
$$

The normalization is:

$$
    p(\mathcal{D}) = \sum^m_{i=1} p(\mathcal{D}|M_i)p(M_i)
$$

The relative probability of a model is:

$$
    \frac{p(M_i|\mathcal{D})}{p(M_j|\mathcal{D})} = \frac{p(\mathcal{D}|M_i)}{p(\mathcal{D}|M_j)} \frac{p(M_i)}{p(M_j)}
$$

The model likelihood for continuous model parameters is given as:

$$
    p(\mathcal{D}|M_i) = \int p(\mathcal{D}|\theta_i,M_i)p(\theta_i|M_i)d\theta_i
$$

For \\( N \\) i.i.d. data points the likelihood takes the form:

$$
    p(\mathcal{D}|M_i) = \int p(\theta_i|M_i)\prod^N_{n=1}p(x^n|\theta_i,M_i)d\theta_i
$$

and the unnormalized log-posterior is:

$$
    \log p(\mathcal{D}|M_i)p(M_i) = \log p(\boldsymbol{\theta}|M) + \sum^N_{n=1}\log p(x^n|\boldsymbol{\theta},M)
$$

## Laplace method
For computing the model likelihood the integral is usually intractable and has to be approximated. The Laplace method to approximate the model log-likelihood for a Gaussian-like posterior is to find \\( \boldsymbol{\theta}^* \\) via MAP and to fit a Gaussian to this point in the parameter space based on the local curvature:

$$
    \boldsymbol{\theta}^* = \underset{\boldsymbol{\theta}}{\operatorname{argmax}} \mbox{ }p (\mathcal{D}|\boldsymbol{\theta},M)p(\boldsymbol{\theta}|M)
$$

$$
    \log p(\mathcal{D}|M) \approx \log p(\mathcal{D}|\boldsymbol{\theta}^*,M)+\log p(\boldsymbol{\theta}^*|M)+\frac{1}{2}\log \det (2\pi \textbf{H}^{-1})
$$

Hereby \\( \textbf{H} \\) is the Hessian of the negative log posterior evaluated at \\( \boldsymbol{\theta}^* \\) .

## BIC Approximation
An even simpler method is to crudely approximate the Hessian \\( \textbf{H} \\) by taking \\( \boldsymbol{H}\approx N\boldsymbol{I}_k \\) where  \\( K=\dim(\boldsymbol{\theta}) \\) is the number of model parameters. With this Hessian the Laplace approximation is:

$$
    \log p(\mathcal{D}|M) \approx \log p(\mathcal{D}|\boldsymbol{D}|\boldsymbol{\theta}^*,M)+\log p(\boldsymbol{\theta}^*|M)+\frac{K}{2}(\log 2\pi -\log N)
$$

One can further approximate the prior and take 

$$
( p(\boldsymbol{\theta}|M) = \mathcal{N}(\boldsymbol{\theta}|\boldsymbol{0},\boldsymbol{I}) 
$$

, which penalizes the length of the parameter vector and favorizing a simple model. With that the log-likelihood is:

$$
    \log p(\boldsymbol{D}|M) \approx \log p(\mathcal{D}|\boldsymbol{\theta}^*,M)-\frac{1}{2}(\boldsymbol{\theta}^*)^T\boldsymbol{\theta}^*-\frac{K}{2}\log N
$$

By ignoring the penalty term for large \\( N \\) one arrives at the definition of the **Bayes Information Criterion (BIC)**:

$$
    BIC = K\log N -2\log p(\mathcal{D}|\boldsymbol{\theta}^*,M)
$$

BIC is closely related to the **Akaike Information Criterion (AIC)** which is:

$$
    AIC = 2K-2\log p(\mathcal{D}|\boldsymbol{\theta}^*,M) 
$$
