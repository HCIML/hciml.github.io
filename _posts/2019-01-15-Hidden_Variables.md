---
layout: post
title:  "Basic: Hidden Variables"
categories: [ Graphical models ]
author: ghost
published: true
image: assets/images/4.jpeg
---

New components of a model are hidden/latent variables \\( x_h \\) , i.e. parameters which are essential for the model but potentially cannot be observed due and missing variables which typically miss at random due to imperfection of data and are simply modelled by an index function \\( \textbf{I}_m \\). 

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
