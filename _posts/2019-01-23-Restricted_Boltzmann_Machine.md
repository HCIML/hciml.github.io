---
layout: post
title:  "Intermediate: Restricted Boltzmann Machine"
categories: [ neural models ]
author: ghost
image: assets/images/7.jpg
---

A Boltzmann machine is a MN, a particular log-linear MRF (energy function is linear in its free parameters), on binary variables with the probability distribution of the form \\( p(x) = \frac{1}{Z}e^{-E(v,h)} \\):

$$  
    p(x) = \frac{1}{Z}(\textbf{w},b)e^{\sum_{i<j}w_{ij}x_ix_j+\sum_ib_ix_i}
$$

with the interactions \\( w_{ij} \\) (weights) and the biases \\( b_i \\). The graphical model of the Boltzmann machine is an undirected graph with a link between nodes i and j for \\( w_{ij} \neq 0 \\). Consequently, for all but specially constrained W, the graph is multiply-connected and inference will be typically intractable. These are called **Restricted Boltzmann Machines**. 

The simplest RBM is a bipartite graph with shared weights, i.e. two layers, a visible first layer i and a hidden second layer j. In that case the energy function is:

$$ 
    E(v,h) = -\sum_ia_iv_i-\sum_jb_jh_j-\sum_i\sum_jv_iw_{ij}h_j
$$

which is in matrix notation:

$$
    E(v,h) = -a^Tv-b^Th-v^TWh
$$

with \\( a \\) being the biases for the hidden units.
Training is done via **back-propagation**, i.e. the parameters \\( (b, w) \\) are trained in unsupervised manner by comparing the reconstruction, starting with the output neural activations and going backwards in the parametric model, with the initial signal and adjusting the parameters when the **Kullback-Leibler Divergence** between both is large.

The free energy is:

$$
    \mathcal{F}(v) = -b^Tv-\sum_i\log \sum_{h_i} e^{h_i(c_i+W_iv)}
$$

With conditional independence assumption one has:

$$
    p(h|v) = \prod_i p(h_i|v)
$$

$$
    p(v|h) = \prod_jp(v_j|h)
$$

Given binary units \\( v_j \\) and \\( h_i \in \{0,1\} \\) one has:

$$
    p(h_i=1|v) = sign(c_i+W_iv)
$$

$$
    p(v_j=1|h) = sign(b_j+W^T_jh)
$$

With this the free energy is:

$$
    \mathcal{F}(v) = -b^Tv-\sum_i\log (1+e^{(c_i+W_iv)})
$$

The gradients are:

$$
    -\frac{\partial \log p(v)}{\partial W_{ij}} = \mathbb{E}_v [ p(h_i|v)v_j]-v_j^{(i)} sign(W_iv^{(i)}+c_i)
$$

$$
    -\frac{\partial \log p(v)}{\partial c_i} = \mathbb{E}_v[p(h_i|v)]-sign(W_iv^{(i)})
$$

$$
    -\frac{\partial \log p(v)}{\partial b_j} = \mathbb{E}_v[p(v_j|h)]-v_j^{(i)}
$$

## Sampling
RBM uses block Gibbs sampling where \\( h \\) and \\( v \\) are sampled given each other fixed:

$$
    h^{(n+1)}\sim sign (W^Tv^{(n)}+c)
$$

$$
    v^{(n+1)} \sim sign (Wh^{(n+1)}+b)
$$

**Contrastive Divergence** is an approximation technique for speeding up known in the context of RBMs used for the likelihood gradient approximation of the backpropagation algorithm via a short MCMC. It is shown that even one cycle of MCMC is sufficient for convergence of the RBM.
