---
title:  "Why Regularized Auto-Encoders learn Sparse Representation?"
date:   2016-05-29 22:37:00
categories: PaperList
---

This paper has some great insights to offer in design of autoencoders specially "does regularized helps in learning sparse representation of the data?"

Here is the setup: we have an input $$\mathbf{x} \in \mathbf{R}^{d}$$ which is mapped to latent space $$\mathbf{h=s(Wx+b)}$$ via autoencoder where $$\mathbf{s}$$ is encoder activation function, $$\mathbf{W} \in \mathbf{R}^{m\times n}$$ is the weight matrix, and $$\mathbf{b} \in \mathbf{R^m}$$ is the encoder bias and $$\mathbf{h} \in \mathbf{R^m}$$ is hidden representation or outputs of hidden units.

For analysis, paper assumes that decoder is linear i.e. $$\mathbf{W^{T}h}$$ decodes back the encoded hidden representation and loss is squared loss function. Therefore, for learning $$\mathbf{W,b}$$ parameters of autoencoder objective function is:

$$\begin{equation}
\begin{split}
J_{AE} &=\mathbb{E_x}[(\mathbf{x-W^Th})^2] \\
&= \mathbb{E_x}[(\mathbf{x-W^Ts(Wx+b)})^2]\\
\end{split}
\end{equation}$$

>We are interested in sparsity of $$\mathbf{h}$$, hidden representation.

Besides above, paper makes two more assumptions.

>**Assumption 1**: Data is drawn from a distribution $$\mathbf{x} \sim  X$$ for which $$\mathbb{E_x}[\mathbf{x}]=0$$ and $$\mathbb{E_x}[\mathbf{xx^T]=I}$$, where $$\mathbf{I}$$ is identity matrix.

Basically, it assumes that data is whitened which is reasonable for some cases. There is one more assumption from analysis point of view which is needed for derivation of theorems, we will see that later.

Finally, for a give data sample $$\mathbf{x}$$, each hidden unit $$h_j$$ gets activated if pre-activation unit $$a_j$$ is greater than $$\delta$$ threshold:

$$a_j=W_j\mathbf{x}+b_j$$

$$h_j=s(a_j)$$

Let's further 

The idea is to somehow show that pre-activation units $$a_j, \forall j$$ for majority of samples is less than $$\delta$$ threshold. Why we want that? It would mean that     
