---
title:  " Blog: Why Regularized Auto-Encoders learn Sparse Representation?"
date:   2016-05-31 22:37:00
categories: PaperList
---

This paper has some great insights to offer in design of autoencoders. As title suggests, it addresses "whether regularized helps in learning sparse representation of the data theoretically?"

**Here is the setup:** we have an input $$\mathbf{x} \in \mathbf{R}^{d}$$ which is mapped to latent space $$\mathbf{h=s(Wx+b)}$$ via autoencoder where $$\mathbf{s}$$ is encoder activation function, $$\mathbf{W} \in \mathbf{R}^{m\times n}$$ is the weight matrix, and $$\mathbf{b} \in \mathbf{R^m}$$ is the encoder bias and $$\mathbf{h} \in \mathbf{R^m}$$ is hidden representation or outputs of hidden units.

For analysis, paper assumes that decoder is linear i.e. $$\mathbf{W^{T}h}$$ decodes back the encoded hidden representation and loss is squared loss function. Therefore, for learning $$\mathbf{W,b}$$ parameters of autoencoder; objective function is:

$$\begin{equation}
\begin{split}
J_{AE} &=\mathbb{E_x}[(\mathbf{x-W^Th})^2] \\
&= \mathbb{E_x}[(\mathbf{x-W^Ts(Wx+b)})^2]\\
\end{split}
\end{equation}$$

>Now we are interested in sparsity of $$\mathbf{h}$$, hidden representation.

Besides above, paper makes two more assumptions.

>**Assumption 1**: Data is drawn from a distribution $$\mathbf{x} \sim  X$$ for which $$\mathbb{E_x}[\mathbf{x}]=0$$ and $$\mathbb{E_x}[\mathbf{xx^T]=I}$$, where $$\mathbf{I}$$ is identity matrix.

Basically, it assumes that data is whitened which is reasonable for some cases. There is one more assumption from analysis point of view which is needed for the derivation of theorems, we will see that later.

Finally, for a give data sample $$\mathbf{x}$$, each hidden unit $$h_j$$ gets activated if pre-activation unit $$a_j$$ is greater than $$\delta$$ threshold:

$$a_j=W_j\mathbf{x}+b_j$$

$$h_j=s(a_j)$$


 So, the sparsity of $$\mathbf{h}$$ depends upon the value of pre-activation units (whose value further depend upon the input data samples). If the expected value of pre-activation unit is less than threshold over data distribution then the corresponding hidden unit expected output is  zero.

 >The idea is to somehow show that regularization encourages the expected value of pre-activation unit $$a_j, \forall j$$  to reduce on average in autoencoders. This indirectly induces sparsity in $$\mathbf{h}$$, hidden representation.  

 **This should hint us that:** monotonically increasing activation functions would be preferable because we want to decrease hidden unit value as the value of pre-activation unit decreases. On the top of that, if we consider monotonically increasing activation functions with negative saturation at $$0$$, i.e. $$\lim_{a\to-\infty} s(a)=0$$  then we can make sure that lower average pre-activation value implies higher sparsity. For example, consider $$tanh$$ function whose range is $$(-1,1)$$. If we reduce $$a$$ below zero, hidden unit output will move away from $$0$$.

Let's focus on analysis now. Assume we want to compute $$\mathbf{W,b}$$ using gradient descent. Then at $$t^{th}$$ iteration of updating, we will have:

$$a_j^{t}=W_j^{t}\mathbf{x}+b_j^{t}$$

where $$a_j^{t}, W_j^{t}, b_j^{t}$$ are values in $$t^{th}$$ iteration.

Let regularized objective function of autoencoder is expressed as:

$$J_{RAE}=J_{AE}+\lambda R(\mathbf{W,b})$$

where $$R(\mathbf{W,b})$$ is the regularization term and $$\lambda>0$$. Simplifying the main theorem of the paper over here:

>**Theorem 1**: For each iteration: $$\mathbb{E_x}[a_j^{t+1}]<\mathbb{E_x}[a_j^{t}]$$  holds for $$\forall j$$ with probability $$(1-\frac{\sigma^2}{\epsilon^2})$$, if following conditions are met: $$\hspace{5em}\lambda \frac{\partial R}{\partial b_j} > 2\epsilon \sqrt{n}  \|W_j\|$$ and encoding activation function, $$s$$, first derivative is bounded in $$[0,1]$$.


The above theorem shows that  updating $$\mathbf{W,b}$$ along the negative gradient of $$J_{RAE}$$ results in $$\mathbb{E_x}[a_j^{t+1}]<\mathbb{E_x}[a_j^{t}]$$ with high probability if particular regularization condition is met (plus activation function condition). This means certain regularization terms explicitly encourages sparsity in hidden representation.


> **Corollary 1**: Theorem 1 holds for two special cases: \\
>1)  activation function $$s$$ is non-decreasing and regularization term has form $$R=\sum\limits_{j=1}^{m}f(\mathbb{E_x}[h_j])$$ for some monotonically increasing function $$f.$$\\
>2)  activation function $$s$$ is convex plus non-decreasing and regularization term has form $$R=\mathbb{E_x}[\sum\limits_{j=1}^{m}(\frac{\partial h_j}{\partial a_j})^q \|W_j\|^{p}_2]$$ where $$p$$ is natural and $$q$$ whole number.


**We are ready to make some remarks about in practice activation functions!**

>**ReLu**: It satisfies both corollaries. Hence can be used with any regularized of first form but not the second form (because second derivative does not exist). The advantage of ReLU is
that it enforces hard zeros in the learned representations.

>**Softplus**: It satisfies both corollaries and encourages sparsity for both suggested regularization form. But does not produce hard zeros.

>**Sigmoid**: Satisfies only first corollary.  Hence sigmoid is not guaranteed to lead to sparsity when used with second regularizations form.

>**Maxout and Tanh**: Do not satisfies negative saturation property at $$0$$ and hence may or may not lead to sparsity.


**Final remarks about about in practice regularized autoencoders!**

1. De-noising Autoencoder (DAE) has regularization of second form.
2. Contractive Auto-Encoder (CAE) including higher order has  regularization of second form.
3. Marginalized De-noising Auto-Encoder (mDAE) has  regularization of second form.
4. Sparse Auto-Encoder (SAE) has regularization of first form.

> Thus, all above autoencoders encourages sparsity theoretically, if chosen with appropriate activation function.

**References**:

1. Arpit, D., Zhou, Y., Ngo, H., & Govindaraju, V. (2015). Why Regularized Auto-Encoders learn Sparse Representation? ICML2016. [[PDF]](http://arxiv.org/pdf/1505.05561.pdf)

