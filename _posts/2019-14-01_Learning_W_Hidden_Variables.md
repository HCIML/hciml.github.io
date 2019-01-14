---
layout: post
title:  "Intermediate: Learning with Hidden Variables"
categories: [ General, model evaluation ]
author: ghost
featured: true
image: assets/images/4.jpeg
---

New components of a model are hidden/latent variables \\( x_h \)), i.e. parameters which are essential for the model but potentially cannot be observed due and missing variables which typically miss at random due to imperfection of data and are simply modelled by an index function \\( \textbf{I}_m \\). 

In general the probability of a special outcome of \\( x_v,\textbf{I}_m\\) will be:

$$
    p(x_v,\textbf{I}_m|\theta) = \sum_{x_i}p(x_v,x_i,\textbf{I}_m|\theta) =  \sum_{x_i}p(\textbf{I}_m|x_v,x_h,\theta)p(x_v,x_i|\theta)
$$

One can further assume that the generation process for missing variables only depends on visible variables which is called **missing at random (MAR)**:

$$
    p(\textbf{I}_m|x_v,x_h,\theta) = p(\textbf{I}_m|x_v)
$$

With that the probability of the observables of the model is:

$$
    p(x_v,\textbf{I}_m|\theta) = p(\textbf{I}_m|x_v)p(x_v|\theta)
$$

and the parameters can be accessed from the marginal likelihood. MAR is an assumption that cannot be verified statistically.
The **missing completely at random assumption (MCAR)** requires that the generation process of missing variables is completely independent of the model entities:

$$
    p(\textbf{I}_m|x_v,x_h,\theta) = p(\textbf{I}_m)
$$

In MCAR case the model is completely unbiased. MCAR is almost never the case.

## Variational Expectation Maximization

Hidden variables prevent decoupling of marginals. The theoretical discussion in this chapter shows how EM is aimed on replacing the objective function by the lower bound where the coupling is removed.

Consider a single visible variable \\( v \\) and a single hidden variable \\( h \\). One wishes to set \\( \theta \\) for the model \\( \p(v,h|\theta) \) by maximizing the marginal likelihood \\( p(v|\theta) \). The Kullback-Leibler divergence between a 'variational' distribution \\( q(h|v) \\) and the model is:

$$
    KL(q(h|v)|p(h|v,\theta)) = <\log q(h|v) - \log p(h|v,\theta)>_{q(h|v)} \geq 0 \addtag \addtag
$$

With the use of:

$$
    p(h|v,\theta) = \frac{p(h,v|\theta)}{p(v|\theta)} \addtag
$$

one can rewrite:

$$
    KL(q(h|v)|p(h|v,\theta)) = <\log q(h|v)>_{q(h|v)} - <\log p(h,v|\theta)>_{q(h|v)}+\log p(v|\theta) \geq 0  \addtag
$$

This gives a lower bound on the marginal likelihood for a single training example:

$$
    \log p(v|\theta) \geq -<\log q(h|v)>_{q(h|v)}+<\log p(h,v|\theta)>_{q(h|v)} = \text{Entropy} + \text{Energy} \addtag
$$

The energy term is also called the 'expected complete data log likelihood'. Having more than one data point, i.i.d. data \\( \mathcal{V} \\), the full log likelihood is:

$$
    \log p(\mathcal{V}|\theta) = \sum_{n=1}^N \log p(v^n|\theta)  \addtag
$$

And the lower bound is:

$$
    -\sum^N_{n=1}<\log q(h^n|v^n)>_{q(h^n|v^n)}+\sum^^N_{n=!}<\log p(h^n,v^n|\theta)>_{q(h^n|v^n)} := B(\mathcal{Q},\theta) \addtag
$$

where \\( \mathcal{Q} \\) is a set of variational distributions and the bound is again the sum of an energy term and an entropy term. The optimization of the lower bound is done via first optimizing the equation w.r.t. \\( \mathcal{Q} \\) keeping \\( \theta \\) constant \textbf{(E-step)} and then w.r.t. \\( \theta \\) keeping \\( \mathcal{Q} \\) constant **(M-step)**. The two steps are repeated until convergence. Since during the E-step \\( q \\) is fixed, it only maximizes the energy term (called classical EM). The steps are repeated until convergence. The fully optimal setting is:

$$
    q(h^n|v^n) = p(h^n|v^n,\theta) \implies \log p(\mathcal{V}|\theta) = B(\mathcal{Q},\theta) \addtag
$$

Due to the local optimization the global maximum can not always be reached. By design of EM the lower bound is never decreased. A positive side effect is that the log likelihood is optimized too, i.e. is higher with the updated \\( \theta \\).

## EM for Belief networks

Given the general form of a BN:

$$
    p(x) = \prod_i p(x_i|pa(x_i))
$$

and N variables \\( x=(v,h) \\) with its visible and hidden part the **energy term** is:

$$
    \sum_n <\log p(x^n)>_{q_t(h^n|v^n)} = \sum_n\sum_i<\log p(x^n_i|pa(x_i^n))>_{q_t(h^n|v^n)} = N<\log p(x)>_{q_t(x)}
$$

This expression is obtained by using the notation:

$$
    q_t^n(x) := q_t(h^n|v^n)\delta (v,v^n)
$$

and defining a \textbf{mixture distribution}:

$$
    q_t(x) := \frac{1}{N}\sum^N_{n=1}q_t^n(x) 
$$

where \\( t \\) is the iteration index. The E-step is (without derivation):

$$
    q_t(h^n|v^n)=p^{old}(h^n|v^n)
$$

The M-step is:

$$
    p^{new}(x_i|pa(x_i))=\frac{\sum_nq^n_t(x_i,pa(x_i))}{\sum_{n'} q^{n'}_t (pa(x_i))}
$$

## EM for Markov networks

Given the general form of a MN:

$$
    p(v,h|\theta) = \frac{1}{Z(\theta)}\prod_c\phi_c(h,v|\theta_c), \mbox{ } Z(\theta) = \sum_{v,h}\prod^C_{c=1}\phi_c(h,v|\theta_c), \mbox{ } \theta = (\theta_1,...,\theta_C)
$$

with clique parameters \\( \theta_c \\), one finds the variational bound as being:

$$
    \log p(v|\theta)\geq -<\log p(x)>_{p(x)}+\sum_c<\log \phi_c(h,v|\theta_c)>_{q(h)}-\log Z(\theta)
$$

where the first term is the entropy term. The bound only partially resolves the coupling, since the parameters are still coupled in the normalization term. One technique is to also put a bound on the normalization term. 

## Variational Bayes

The generalization of EM is Variational Bayes. Since it a a Bayesian method, the parameter posterior given a single observation \\( v \\) is:

$$
    p(\theta|v) \propto p(v|\theta)p(\theta)=\sum_h p(v,h|\theta)p(\theta)
$$

In Variational Bayes one approximates by factorization:

$$
    p(h,\theta|v) \approx q(h)q(\theta)
$$

The factors \\( q(h) \\), \\( q(\theta) \\) are found by minimizing the KL divergence between \\( p(h,\theta|v) \\) and \\( q(h)q(\theta) \\):

$$
    KL = -<\log q(h)>_{q(h)}+<\log q(\theta)>_{q(\theta)}-<\log p(h,\theta|v)>_{q(h)q(\theta)} \geq 0
$$

this gives the bound:

$$
    \log p(v) \geq -<\log q(h)>_{q(h)}-<\log q(\theta)>_{q(\theta)}+<\log p(v,h,\theta)>_{q(h)q(\theta)}
$$

with the E-step

$$
    q^{new}(h) = \underset{q(h)}{\operatorname{argmin}}  KL (q(h)q^{old}(\theta)|p(h,\theta|v))
$$

and the M-step

$$
    q^{new}(\theta) = \underset{q(\theta)}{\operatorname{argmin}}  KL (q^{new}(h)q(\theta)|p(h,\theta|v))
$$

One can find that 

$$
q^{new}(h) \propto \exp <\log p(v,h|\theta)>_{q(\theta)}
$$

and 

$$
q^{new}(\theta) \propto p(\theta)\exp<\log p(v,h|\theta)>_{q(h)}
$$

For i.i.d. data one obtains the bound for the whole data set as:

$$
    \log p(V) = \sum_n \log p(v^n)
$$

EM is a special case of Variational Bayes with a flat prior  \\(p(\theta) = const. \\) and delta function approximation of the parameter posterior \\( q(\theta) = \delta (\theta, \theta^*)\\) .
