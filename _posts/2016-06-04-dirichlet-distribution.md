---
title:  " Blog: Dirichlet Distributions."
date:   2016-06-04 00:01:00
categories: Tutorials
---

Dirichlet Distribution is *a distribution over distributions*. More specifically, it is a distribution over *pmfs (probability mass functions)*. You can imagine, as if there is a bag of $$L$$ dices, and each dice has a corresponding pmf (related to six possible outcomes). Now picking a dice is like sampling a particular pmf from a distribution. The probability of picking a dice, which results in a pmf, comes from the Dirichlet distribution $$Dir(\boldsymbol\alpha)$$.

Let $$\mathbf{q}=[q_1,q_2,...,q_k]$$ be a pmf (or a point in simplex $$\in$$ $$\mathbf{R}^{k}$$), where $$\sum\limits_{j=1}^{k}q_j=1$$ and $$\mathbf{q} \sim Dir(\boldsymbol\alpha)$$. Here $$\boldsymbol\alpha$$ is dirichlet parameter $$\boldsymbol\alpha=[\alpha_1,\alpha_2,...,\alpha_k]$$ and $$\alpha_0=\sum\limits_{j=1}^{k}\alpha_j$$. Then, the probability density of $$\mathbf{q}$$ is given by:

$$f(q,\boldsymbol\alpha)=p(q|\boldsymbol\alpha)=\frac{\Gamma(\alpha_0)}{\prod\limits_{j=1}^{k}\Gamma(\alpha_j)}\prod\limits_{j=1}^{k}q_j^{\alpha_j-1}$$

**Graphical Model:**

Suppose $$\{x_i\}$$ is a set of samples drawn from $$i^{th}$$ pmf where $$i\in[1,L]$$. For eg. $$\{x_i\}$$ could be a sequence of outputs of a dice $$\{1,2,1,3,4,3,6,..\}$$. Let $$\mathbf{q}_1,\mathbf{q}_2,...,\mathbf{q}_L$$ are $$L$$ pmfs. Then:

$$\begin{equation}
\begin{split}
p(\{x_i\}|\boldsymbol\alpha)&=\int p(\{x_i\},\mathbf{q}_i|\boldsymbol\alpha)d\mathbf{q}_i\\
&=\int p(\{x_i\}|\mathbf{q}_i,\boldsymbol\alpha) p(\mathbf{q}_i|\boldsymbol\alpha)  d\mathbf{q}_i\\
&=\int p(\{x_i\}|\mathbf{q}_i) p(\mathbf{q}_i|\boldsymbol\alpha)  d\mathbf{q}_i\\
\end{split}
\end{equation}$$

Let $$n_{ij}$$ be the number of outcomes in $$\{x_i\}$$ sequence of samples that is equal to $$j^{th}$$ event where $$j\in[1,k]$$, and let $$n_i = \sum\limits_{j=1}^{k} n_{ij}$$.

$$p(\{x_i\}|\mathbf{q}_i)=\frac{n_i!}{\prod\limits_{j=1}^{k}n_{ij}!} \prod\limits_{j=1}^{k} q_{ij}^{n_{ij}}$$

Therefore,

$$\begin{equation}
\begin{split}
p(\{x_i\}|\boldsymbol\alpha)&=\int \frac{n_i!}{\prod\limits_{j=1}^{k}n_{ij}!} \prod\limits_{j=1}^{k} q_{ij}^{n_{ij}} \times \frac{\Gamma(\alpha_0)}{\prod\limits_{j=1}^{k}\Gamma(\alpha_j)}\prod\limits_{j=1}^{k}q_{ij}^{\alpha_j-1}  d\mathbf{q}_i\\
&=  \frac{ \Gamma(\alpha_0) n_i! }{\prod\limits_{j=1}^{k}   \Gamma(\alpha_j) n_{ij}!}   \int \prod\limits_{j=1}^{k} q_{ij}^{n_{ij}+\alpha_j-1} d\mathbf{q}_i\\
&=\frac{ \Gamma(\alpha_0) n_i! }{\Gamma (\sum\limits_{j=1}^{k} (n_{ij}+ \alpha_j))}  \prod\limits_{j=1}^{k} \frac{ \Gamma(n_{ij}+\alpha_j) }{\Gamma(\alpha_j) n_{ij}!}\\
\end{split}
\end{equation}$$


>$$p(\{x_i\}|\boldsymbol\alpha)=\frac{ \Gamma(\alpha_0) n_i! }{\Gamma (\sum\limits_{j=1}^{k} (n_{ij}+ \alpha_j))}  \prod\limits_{j=1}^{k} \frac{ \Gamma(n_{ij}+\alpha_j) }{\Gamma(\alpha_j) n_{ij}!}$$


1. Bela A. Frigyik, Amol Kapila, and Maya R. Gupta. "Introduction to the Dirichlet Distribution and Related
Processes". [[PDF]](http://mayagupta.org/publications/FrigyikKapilaGuptaIntroToDirichlet.pdf)