---
layout: post
title:  "Intermediate: Modelling with Hidden Variables"
categories: [ Graphical models ]
author: ghost
featured: false
published: true
image: assets/images/5.jpeg
---

Hidden or latent variables \\( x_h \\) are parameters which are essential for the model but potentially cannot be observed due and missing variables which typically miss at random due to imperfection of data and are simply modelled by an index function \\( \textbf{I}_m \\). 

In general the probability of a special outcome of \\( x_v,\textbf{I}_m\\) will be:

$$ \small

    p(x_v,\textbf{I}_m \vert \theta) = \sum_{x_i}p(x_v,x_i,\textbf{I}_m \vert \theta) =  \sum_{x_i}p(\textbf{I}_m \vert x_v,x_h,\theta)p(x_v,x_i \vert \theta)
$$

One can further assume that the generation process for missing variables only depends on visible variables which is called **missing at random (MAR)**:

$$ \small

    p(\textbf{I}_m \vert x_v,x_h,\theta) = p(\textbf{I}_m \vert x_v)
$$

With that the probability of the observables of the model is:

$$ \small

    p(x_v,\textbf{I}_m \vert \theta) = p(\textbf{I}_m \vert x_v)p(x_v \vert \theta)
$$

and the parameters can be accessed from the marginal likelihood. MAR is an assumption that cannot be verified statistically.
The **missing completely at random assumption (MCAR)** requires that the generation process of missing variables is completely independent of the model entities:

$$ \small

    p(\textbf{I}_m \vert x_v,x_h,\theta) = p(\textbf{I}_m)
$$

In MCAR case the model is completely unbiased. MCAR is almost never the case.

## Variational Expectation Maximization
Hidden variables prevent decoupling of marginals. The theoretical discussion in this chapter shows how Variational Expectation Maximization is aimed on replacing the objective function by the lower bound where the coupling is removed.

Consider a single visible variable \\( v \\) and a single hidden variable \\( h \\). One wishes to set \\( \theta \\) for the model \\( p(v,h \vert \theta) \\) by maximizing the marginal likelihood \\( p(v \vert \theta) \\). The Kullback-Leibler divergence between a 'variational' distribution \\( q(h \vert v) \\) and the model is:

$$ \small

    KL(q(h \vert v)|p(h \vert v,\theta)) = <\log q(h \vert v) - \log p(h \vert v,\theta)>_{q(h \vert v)} \geq 0
$$

With the use of:

$$ \small

    p(h \vert v,\theta) = \frac{p(h,v \vert \theta)}{p(v \vert \theta)}
$$

one can rewrite:

$$ \small

    KL(q(h \vert v)|p(h \vert v,\theta)) = <\log q(h \vert v)>_{q(h \vert v)} - <\log p(h,v \vert \theta)>_{q(h \vert v)}+\log p(v \vert \theta) \geq 0  
$$

This gives a lower bound on the marginal likelihood for a single training example:

$$ \small

    \log p(v \vert \theta) \geq -<\log q(h \vert v)>_{q(h \vert v)}+<\log p(h,v \vert \theta)>_{q(h \vert v)} = \text{Entropy} + \text{Energy} 
$$

The energy term is also called the 'expected complete data log likelihood'. Having more than one data point, i.i.d. data \\( \mathcal{V} \\), the full log likelihood is:

$$ \small

    \log p(\mathcal{V} \vert \theta) = \sum_{n=1}^N \log p(v^n \vert \theta)  
$$

And the lower bound is:

$$ \small

    -\sum^N_{n=1}<\log q(h^n \vert v^n)>_{q(h^n \vert v^n)}+\sum^N_{n=!} < \log p(h^n,v^n \vert \theta) >_{q(h^n \vert v^n)} := B(\mathcal{Q},\theta) 
$$

where \\( \mathcal{Q} \\) is a set of variational distributions and the bound is again the sum of an energy term and an entropy term. The optimization of the lower bound is done via first optimizing the equation w.r.t. \\( \mathcal{Q} \\) keeping \\( \theta \\) constant **(E-step)** and then w.r.t. \\( \theta \\) keeping \\( \mathcal{Q} \\) constant **(M-step)**. The two steps are repeated until convergence. Since during the E-step \\( q \\) is fixed, it only maximizes the energy term (called classical EM). The steps are repeated until convergence. The fully optimal setting is:

$$ \small

    q(h^n \vert v^n) = p(h^n \vert v^n,\theta) \implies \log p(\mathcal{V} \vert \theta) = B(\mathcal{Q},\theta) 
$$

Due to the local optimization the global maximum can not always be reached. By design of EM the lower bound is never decreased. A positive side effect is that the log likelihood is optimized too, i.e. is higher with the updated \\( \theta \\).

## EM for Belief networks

Given the general form of a BN:

$$ \small

    p(x) = \prod_i p(x_i \vert pa(x_i))
$$

and N variables \\( x=(v,h) \\) with its visible and hidden part the **energy term** is:

$$ \small

    \sum_n <\log p(x^n)>_{q_t(h^n \vert v^n)} = \sum_n\sum_i<\log p(x^n_i \vert pa(x_i^n))>_{q_t(h^n \vert v^n)} = N<\log p(x)>_{q_t(x)}
$$

This expression is obtained by using the notation:

$$ \small

    q_t^n(x) := q_t(h^n \vert v^n)\delta (v,v^n)
$$

and defining a **mixture distribution**:

$$ \small

    q_t(x) := \frac{1}{N}\sum^N_{n=1}q_t^n(x) 
$$

where \\( t \\) is the iteration index. The E-step is (without derivation):

$$ \small

    q_t(h^n \vert v^n)=p^{old}(h^n \vert v^n)
$$

The M-step is:

$$ \small

    p^{new}(x_i \vert pa(x_i))=\frac{\sum_nq^n_t(x_i,pa(x_i))}{\sum_{n'} q^{n'}_t (pa(x_i))}
$$

## EM for Markov networks

Given the general form of a MN:

$$ \small

    p(v,h \vert \theta) = \frac{1}{Z(\theta)}\prod_c\phi_c(h,v \vert \theta_c), \mbox{ } Z(\theta) = \sum_{v,h}\prod^C_{c=1}\phi_c(h,v \vert \theta_c), \mbox{ } \theta = (\theta_1,...,\theta_C)
$$

with clique parameters \\( \theta_c \\), one finds the variational bound as being:

$$ \small

    \log p(v \vert \theta)\geq -<\log p(x)>_{p(x)}+\sum_c<\log \phi_c(h,v \vert \theta_c)>_{q(h)}-\log Z(\theta)
$$

where the first term is the entropy term. The bound only partially resolves the coupling, since the parameters are still coupled in the normalization term. One technique is to also put a bound on the normalization term. 
