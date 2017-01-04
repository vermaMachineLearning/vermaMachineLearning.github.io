---
title:  " Blog: Bayesian Sets."
date:   2016-06-02 00:01:00
categories: Tutorials
---



 
**Bayesian Sets Graphical Model:**
<div style="text-align:center" ><img src ="{{ site.baseurl }}/images/bayesian-sets.svg"  style="width: 35%; height: 35%"/></div>

Bayesian sets are simple graphical models used to expand a set. For instance, suppose you are given a set of few words (or items) $$S=\{cat, dog, lion, ...\}$$ which we refer as "seeds" and we wish to expand this set to include all similiar words from the given text corpus. Then, we can employ Bayesian sets which rank each item based on its importance of belonging to seed set. 

Let $$D$$ be a data set of items, and $$\mathbf{x} \in D$$ be an item from this set. Assume the user provides a query set $$D_c$$ which is a small subset of $$D$$, bayesian sets rank each item   by $$score(\mathbf{x})$$. This probability ratio  can be interpreted as the ratio of the joint probability of observing $$x$$ and $$D_c$$ to the probability of independently observing $$x$$ and $$D_c$$. 

>$$score(\mathbf{x})=\frac{p(\mathbf{x}\vert D_c)}{p(\mathbf{x})}= \frac{p(\mathbf{x}, D_c)}{p(\mathbf{x})p(D_c)}=\frac{\int p(\mathbf{x},\theta \vert D_c ) d\theta}{\int p(\mathbf{x},\theta) d\theta}=\frac{\int p(\mathbf{x} \vert \theta, D_c ) p(\theta \vert D_c) d\theta}{\int p(\mathbf{x} \vert \theta) p(\theta )d\theta}$$

Assume that the parameterized model is $$p(\mathbf{x} \vert \theta)$$ where $$\theta$$ are the parameters as shown in figure above. Here, we assume that $$\mathbf{x}$$ is represented by a binary feature vector and $$\theta_j$$ is the weight associated with feature $$j$$. Then,

For each $$\mathbf{x}_i$$ (Note: $$\mathbf{x}$$ vector $$j^{th}$$ component is $$x_{.j}$$ and bold letters are vectors):

$$p(\mathbf{x}_i\vert \boldsymbol\theta)=\prod\limits_{j=1}^{J} \theta_{j}^{x_{ij}} (1-\theta_j)^{1-x_{ij}}$$

The conjugate prior for the parameters of a Bernoulli distribution is the Beta distribution:

$$\begin{equation}
\begin{split}
p(\boldsymbol\theta\vert \boldsymbol\alpha, \boldsymbol\beta) &=\prod\limits_{j=1}^{J} \frac{\Gamma(\alpha_j+\beta_j)}{\Gamma(\alpha_j)\Gamma(\beta_j)}	 \theta_{j}^{\alpha_j-1} (1-\theta_j)^{\beta_j-1} \\
p(\mathbf{x} \vert \boldsymbol\alpha, \boldsymbol\beta) &=\int p(\mathbf{x} \vert \theta, \boldsymbol\alpha, \boldsymbol\beta) p(\theta )d\theta=\int p(\mathbf{x} \vert \theta) p(\theta \vert \boldsymbol\alpha, \boldsymbol\beta )d\theta \\
&=\int \prod\limits_{j=1}^{J} \frac{\Gamma(\alpha_j+\beta_j)}{\Gamma(\alpha_j)\Gamma(\beta_j)}	 \theta_{j}^{\alpha_j-1+x_{.j}} (1-\theta_j)^{\beta_j -x_{.j} } d\theta\\
&= \prod\limits_{j=1}^{J} \frac{\Gamma(\alpha_j+\beta_j)}{\Gamma(\alpha_j)\Gamma(\beta_j)}	 \int_{0}^{1}  \theta_{j}^{\alpha_j-1+x_{.j}} (1-\theta_j)^{\beta_j -x_{.j} } d\theta_j \\
&=\prod\limits_{j=1}^{J} \frac{\Gamma(\alpha_j+\beta_j)}{\Gamma(\alpha_j)\Gamma(\beta_j)} \frac{\Gamma(\alpha_j+x_{.j})\Gamma(\beta_j+1-x_{.j})}{\Gamma(\alpha_j+\beta_j+1)}	\\
p(D_c \vert \boldsymbol\alpha, \boldsymbol\beta)&=\int p(D_c, \theta) d\theta=\int p(D_c \vert \theta)p(\theta \vert \boldsymbol\alpha, \boldsymbol\beta) d\theta \\
&=\int \Big(\prod\limits_{i=1}^{N} p(\mathbf{x}_i\vert \theta)\Big)p(\theta \vert \boldsymbol\alpha, \boldsymbol\beta) d\theta \\
&=\int \Big(\prod\limits_{i=1}^{N}\Big(\prod\limits_{j=1}^{J} \theta_{j}^{x_{ij}} (1-\theta_j)^{1-x_{ij}}\Big)\Big)\prod\limits_{j=1}^{J} \frac{\Gamma(\alpha_j+\beta_j)}{\Gamma(\alpha_j)\Gamma(\beta_j)}	 \theta_{j}^{\alpha_j-1} (1-\theta_j)^{\beta_j-1}d\theta \\
&=\prod\limits_{j=1}^{J} \frac{\Gamma(\alpha_j+\beta_j)}{\Gamma(\alpha_j)\Gamma(\beta_j)}	 \int_{0}^{1}\theta_{j}^{\alpha_j-1+\sum_{i=1}^{N}x_{ij}} (1-\theta_j)^{\beta_j -\sum_{i=1}^{N}x_{.j}+N+1 } d\theta_j \\
&= \prod\limits_{j=1}^{J} \frac{\Gamma(\alpha_j+\beta_j)}{\Gamma(\alpha_j)\Gamma(\beta_j)}   \frac{\Gamma(\widetilde\alpha_j)\Gamma(\widetilde\beta_j)}{\Gamma(\widetilde\alpha_j+\widetilde\beta_j)} \\
&\text{where},  \widetilde\alpha_j= \alpha_j+\sum_{i=1}^{N}x_{ij} , \hspace{1em} \widetilde\beta_j=\beta_j+N-\sum_{i=1}^{N}x_{ij}\\
\end{split}
\end{equation}$$


$$\begin{equation}
\begin{split}
p(\mathbf{x}\vert D_c,\mathbf{\alpha,\beta})&= \int p(\mathbf{x} \vert \theta) p(\theta\vert D_c,\alpha,\beta)d\theta \\
&=\int p(\mathbf{x} \vert \theta)\frac{p(D_c \vert \theta, \alpha,\beta)p(\theta \vert \alpha,\beta)}{p(D_c\vert \alpha,\beta)} d\theta  \\
&= \frac{1}{p(D_c\vert \alpha,\beta)} \int \Big(\prod\limits_{j=1}^{J} \theta_{j}^{x_{.j}} (1-\theta_j)^{1-x_{.j}}\Big) \times \Big(\prod\limits_{i=1}^{N}p(x_i\vert \theta)\Big) \times \\
&\Big(\prod\limits_{j=1}^{J} \frac{\Gamma(\alpha_j+\beta_j)}{\Gamma(\alpha_j)\Gamma(\beta_j)}	 \theta_{j}^{\alpha_j-1} (1-\theta_j)^{\beta_j-1}\Big) d\theta\\
&=\prod\limits_{j=1}^{J} \frac{\Gamma(\widetilde\alpha_j+\widetilde\beta_j)}{\Gamma(\widetilde\alpha_j)\Gamma(\widetilde\beta_j)} \int \theta_j^{x_{.j}+\sum_{i=1}^{N}x_{ij}+\alpha_j-1}(1-\theta_j)^{x.j+N-\sum_{i=1}^{N}x_{ij}+\beta_j}d\theta \\
&=\prod\limits_{j=1}^{J} \frac{\Gamma(\alpha_j+\beta_j+N)}{\Gamma(\widetilde\alpha_j)\Gamma(\widetilde\beta_j)}\frac{\Gamma(\widetilde\alpha_j+x.j)\Gamma(\widetilde\beta_j+1-x_{.j})}{\Gamma(\alpha_j+\beta_j+N+1)} \\
\end{split}
\end{equation}$$

$$\begin{equation}
\begin{split}
score(\mathbf{x})&=\frac{p(\mathbf{x}\vert D_c,\mathbf{\alpha,\beta})}{p(\mathbf{x}\vert \mathbf{\alpha,\beta})}= \\
&=\prod\limits_{j=1}^{J} \frac{\Gamma(\alpha_j+\beta_j+N)}{\Gamma(\widetilde\alpha_j)\Gamma(\widetilde\beta_j)}\frac{\Gamma(\widetilde\alpha_j+x.j)\Gamma(\widetilde\beta_j+1-x_{.j})}{\Gamma(\alpha_j+\beta_j+N+1)} \times \\
& \frac{\Gamma(\alpha_j)\Gamma(\beta_j)}{\Gamma(\alpha_j+\beta_j)} \frac{\Gamma(\alpha_j+\beta_j+1)}{\Gamma(\alpha_j+x_{.j})\Gamma(\beta_j+1-x_{.j})} \\
&=\prod\limits_{j=1}^{J} \frac{\alpha_j+\beta_j}{\alpha_j+\beta_j+N} \Big(\frac{\widetilde\alpha_j}{\alpha_j}\Big)^{x_{.j}} \Big(\frac{\widetilde\beta_j}{\beta_j}\Big)^{1-x_{.j}}
\end{split}
\end{equation}$$

The log of the score is linear in features of $$\mathbf{x}$$:

> $$\log score(\mathbf{x})=c+\sum_j q_j x{.j}$$

Thus, bayesian sets essentially performs **feature selection**.  

For models where $$\mathbf{x}$$ is parametrized using exponential families, we have a similar expression but may or may not be linear in features of $$\mathbf{x}$$.

**Exponential Families:**

$$\begin{equation}
\begin{split}
p(\mathbf{x}\vert \theta)&=f(\mathbf{x})g(\theta)e^{\theta^{T} u(\mathbf{x})} \\
p(\theta \vert \eta,\nu)&= h(\eta,\nu)g(\theta)^{\eta}e^{\theta^{T}\nu} \\
h(\eta,\nu)&=\frac{1}{\int g(\theta)^{\eta}e^{\theta^T\nu}d\theta}\\
p(\mathbf{x} \vert \eta,\nu)&=\int f(\mathbf{x})g(\theta)e^{\theta^T u(\mathbf{x})}h(\eta,\nu)g(\theta)^{\eta}e^{\theta^T\nu}d\theta\\
&=\frac{f(\mathbf{x}) h(\eta,\nu)}{h(\eta+1,u(\mathbf{x})+\nu)} \\
p(D_c \vert \eta,\nu)&=\int \Big(\prod\limits_{i=1}^{N} f(\mathbf{x_i})g(\theta)e^{\theta^{T} u(\mathbf{x_i})}\Big) h(\eta,\nu)g(\theta)^{\eta}e^{\theta^T\nu}d\theta\\
&=\frac{h(\eta,\nu)\Big(\prod\limits_{i=1}^{N} f(\mathbf{x_i})\Big)}{h(\eta+N,\sum\limits_{i=1}^{N}u(\mathbf{x})+\nu)}\\
p(\mathbf{x} \vert D_c, \eta, \nu)&= \int f(\mathbf{x})g(\theta)e^{\theta^T u(\mathbf{x})}\Big(\prod\limits_{i=1}^{N} f(\mathbf{x_i})g(\theta)e^{\theta^{T} u(\mathbf{x_i})}\Big)  \times \\
& h(\eta,\nu)g(\theta)^{\eta}e^{\theta^T\nu}d\theta\\
&=\frac{h(\eta,\nu)f(x)\Big(\prod\limits_{i=1}^{N} f(\mathbf{x_i})\Big)}{p(D_c \vert \eta,\nu) h{(\eta+N+1,\nu+\sum_{i=1}^{N}u(\mathbf{x_i})+u(\mathbf{x})})}\\
score(\mathbf{x})&=\frac{ h(\eta+1,\nu+u(\mathbf{x}))  h(\eta+N,\nu+\sum\limits_{i=1}^{N}u(\mathbf{x}_i))}{h(\eta,\nu) h{(\eta+N+1,\nu+\sum_{i=1}^{N}u(\mathbf{x_i})+u(\mathbf{x})})}
\end{split}
\end{equation}$$



**References:**

1. Ghahramani, Zoubin, and Katherine A. Heller. "Bayesian sets." NIPS. Vol. 2. 2005. [[PDF]](https://papers.nips.cc/paper/2817-bayesian-sets.pdf)
2. Verma, Saurabh, and Estevam R. Hruschka Jr. "Coupled bayesian sets algorithm for semi-supervised learning and information extraction." Joint European Conference on Machine Learning and Knowledge Discovery in Databases. Springer Berlin Heidelberg, 2012. [[PDF]](http://rtw.ml.cmu.edu/papers/ecml12-verma.pdf)