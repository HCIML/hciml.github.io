---
layout: post
title:  "Basic: Learning A Gaussian"
categories: [ General, model evaluation ]
author: ghost
image: assets/images/3.jpg
---
The multivariate Gaussian is defined as: 

$$
	\mathcal{N}(\textbf{x}|\boldsymbol{\mu}, \boldsymbol{\Sigma}) = \frac{1}{\sqrt{\det (2\pi) \boldsymbol{\Sigma}}}e^{-\frac{1}{2}(\textbf{x}-\boldsymbol{\mu})^T\boldsymbol{\Sigma}^{-1}(\textbf{x}-\boldsymbol{\mu})}
$$

with the **precision matrix** \\( \boldsymbol{\Sigma}^{-1} \)) . 

## Maximum Likelihood training (i.i.d.)

**Optimal** \\( \boldsymbol{\mu} \\) **:**

$$
	\nabla_{\boldsymbol{\mu}}L(\boldsymbol{\mu},\boldsymbol{\Sigma}) = 0 
$$

\\( \boldsymbol{\mu}^* \\) is the sample mean.

**Optimal** \\( \boldsymbol{\Sigma} \\) **:**
is the sample covariance.

## Bayesian training (i.i.d., univariate)

For the Bayesian training we need the posterior, conjugate prior and Likelihood. The Likelihood \\( \mathcal{N}(\mathcal{X}|mu,\sigma^2) \\) is a Gaussian. The posterior is:

$$
	p(\mu,\sigma^2|\mathcal{X}) \propto p(\mathcal{X}|\mu,\sigma^2)p(\mu,\sigma^2)=p(\mathcal{X}|\mu, \sigma^2)p(\mu|\sigma^2)p(\sigma^2) 
$$

The conjugate prior is:

$$
p(\mu|\sigma^2)p(\sigma^2) 
$$

The convenient choice for the prior on the mean \\( p(\mu|\sigma^2) \\) is that it is a Gaussian centered around a \\( \mu_0 \\):

$$
	p(\mu|\sigma^2) = p(\mu|\mu_0,\sigma_0^2) = \mathcal{N}(\mu|\mu_0,\sigma_0^2) 
$$

The posterior is correspondingly:

$$
	p(\mu,\sigma^2|\mathcal{X}) \propto \mathcal{N}(\mathcal{X}|\mu,\sigma^2)\mathcal{N}(\mu|\mu_0,\sigma_0^2)p(\sigma^2) 
$$

With the constraint \\( \sigma_0^2 = \text{const.}\cdot\sigma^2 \\) one finds for the \\( p(\sigma^2) \\) part of the prior, that the Inverse-Gamma distribution is conjugate. Therefore the whole prior should take the **Gauss-Inverse-Gamma** form and has a Gauss-Inverse-Gamma conjugate posterior.
