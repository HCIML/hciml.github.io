---
layout: post
title:  "Intermediate: Variational Bayes"
categories: [ Graphical models ]
author: ghost
published: false
image: assets/images/6.jpeg
---

The generalization of EM is Variational Bayes. Since it a a Bayesian method, the parameter posterior given a single observation \\( v \\) is:

$$ \small

    p(\theta \vert v) \propto p(v \vert \theta)p(\theta)=\sum_h p(v,h \vert \theta)p(\theta)
$$

In Variational Bayes one approximates by factorization:

$$ \small

    p(h,\theta \vert v) \approx q(h)q(\theta)
$$

The factors \\( q(h) \\), \\( q(\theta) \\) are found by minimizing the KL divergence between \\( p(h,\theta \vert v) \\) and \\( q(h)q(\theta) \\):

$$ \small

    KL = -<\log q(h)>_{q(h)}+<\log q(\theta)>_{q(\theta)}-<\log p(h,\theta|v)>_{q(h)q(\theta)} \geq 0
$$

this gives the bound:

$$ \small

    \log p(v) \geq -<\log q(h)>_{q(h)}-<\log q(\theta)>_{q(\theta)}+<\log p(v,h,\theta)>_{q(h)q(\theta)}
$$

with the E-step

$$ \small

    q^{new}(h) = \underset{q(h)}{\operatorname{argmin}}  KL (q(h)q^{old}(\theta) \vert p(h,\theta \vert v))
$$

and the M-step

$$ \small

    q^{new}(\theta) = \underset{q(\theta)}{\operatorname{argmin}}  KL (q^{new}(h)q(\theta) \vert p(h,\theta \vert v))
$$

One can find that 

$$ \small

    q^{new}(h) \propto \exp <\log p(v,h \vert \theta)>_{q(\theta)}

$$

and 

$$ \small

    q^{new}(\theta) \propto p(\theta)\exp<\log p(v,h \vert \theta)>_{q(h)}

$$

For i.i.d. data one obtains the bound for the whole data set as:

$$ \small

    \log p(V) = \sum_n \log p(v^n)
    
$$

EM is a special case of Variational Bayes with a flat prior  \\(p(\theta) = const. \\) and delta function approximation of the parameter posterior \\( q(\theta) = \delta (\theta, \theta^*)\\) .
