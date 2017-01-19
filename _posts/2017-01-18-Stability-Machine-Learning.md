---
title:  " Blog: Stability of Learning Algorithms."
date:   2017-01-18 00:01:00
categories: Tutorials
---

This is walk-through on how  to derive generalization bounds based on the stability of algorithms. Understanding the proof helps  to extend or modify the results according to your needs. For instance, I had a loss function which was unbounded, and since to apply generalization results your loss function must be bounded, I was stuck. So, this my effort  to state the proof as clear as possible along with the assumptions needed.


**Setup**: Let $$\mathbf{X} \in \mathbb{R}^d, \mathbf{Y} \in \mathbb{R}$$ and $$S$$ be a training set $$S=\{\mathbf{z}_1=(\mathbf{x}_1,y_1),\mathbf{z}_2=(\mathbf{x}_2,y_2),...,\mathbf{z}_m=(\mathbf{x}_m,y_m)\}$$. Let $$A_S$$ be a symmetric learning algorithm i.e.  does not depend on the order of the data points in the training set. We define the following two more notations as follows:

Removing $$i^{th}$$ data point in the set $$S$$.

$$S^{\backslash i}=\{\mathbf{z}_1,....,\mathbf{z}_{i-1},\mathbf{z}_{i+1},.....,\mathbf{z}_m \}$$

Replacing $$i^{th}$$ data point in the set $$S$$ by $$\mathbf{z}_{i}^{'}$$.

$$S^{i}=\{\mathbf{z}_1,....,\mathbf{z}_{i-1},\mathbf{z}_{i}^{'},\mathbf{z}_{i+1},.....,\mathbf{z}_m \}$$

Let $$D$$ be an unknown distribution from which $$\mathbf{z}_1,....,\mathbf{z}_m$$ data points are sampled to form a training set $$S$$. Here, we assume all samples (including the replacement sample) are i.i.d. unless mentioned otherwise. Let $$\mathbf{E}_S[f]$$ denotes the expectation of the function $$f$$ when  $$m$$  samples are drawn from the distribution $$D$$ to form $$S$$. Similarly, let $$\mathbf{E}_z[f]$$ denotes the expectation of the function $$f$$ when  $$\mathbf{z}$$ is sampled  according to the distribution $$D$$.

We define the generalization error or risk $$R(A_S)$$ as follows:

$$R(A_S)=\mathbf{E}_z[\ell(A_S,\mathbf{z})]=\int \ell(A_S,\mathbf{z}) p(\mathbf{z}) d\mathbf{z}$$

Empirical error $$R_{emp}(A_S)$$ is defined as follows:

$$R_{emp}(A_S):=\frac{1}{m}\sum\limits_{j=1}^{m}\ell(A_S,\mathbf{z}_j)$$

$$\implies R_{emp}(A_{S^{  i}})=\frac{1}{m}\sum\limits_{j=1, j \neq i}^{m}\ell(A_{S^{ i}},\mathbf{z}_j)+\frac{1}{m}\ell(A_{S^{ i}},\mathbf{z}_i^{'})$$


$$R_{emp}(A_{S^{\backslash i}}):=\frac{1}{m}\sum\limits_{j=1, j \neq i}^{m}\ell(A_{S^{\backslash i}},\mathbf{z}_j)$$


**Generalization Bounds based on Stability of Learning Algorithms**:

>Let's assume that our algorithm $$A_S$$ has uniform stability $$\beta$$, if   it satisfies $$\forall i\in\{1,m\}$$,
>
>$$ \underset{S,z}{\sup}|\ell(A_S,\mathbf{z}) -\ell(A_{S^{\backslash i}},\mathbf{z})| \leq \beta $$

To derive generalization bounds for uniform stable algorithms, we are going to mainly use McDiarmid's inequality. Let $$\mathbf{X}$$ be some set and $$f:\mathbf{X}^{m} \rightarrow R$$, then inequality is given as (more detail [here](http://web.eecs.umich.edu/~cscott/past_courses/eecs598w14/notes/09_bounded_difference.pdf)),
>$$ if \underset{x_1,..,x_i,..,x_m,x_i^{'}}{\sup}|f(x_1,..,x_i,..,x_m)-f(x_1,..,x_i^{'},..,x_m)| \leq c_i $$
>
>$$ =\underset{x_1,..,x_i,..,x_m,x_i^{'}}{\sup}|f_S-f_{S^{i}}| \leq c_i \hspace{1em},\forall i $$
>
>$$ \implies P\Big( f(S)-\mathbf{E}_S[f(S^{i})] \geq \epsilon \Big) \leq e^{-\frac{2 \epsilon^2}{\sum_{i=1}^{m}c_i^2}}$$


Strategy is to bound $$\vert f_S-f_{S^{i}} \vert$$ using $$ \vert f_S-f_{S^{\backslash i}} \vert$$, $$ \vert f_{S^{i}}-f_{S^{\backslash i}} \vert$$.  We will derive some lemmas that would be helpful to compute variables needed for applying McDiarmid's inequality.

Since, samples are i.i.d., we have,

$$\begin{equation}
\begin{split}
\mathbf{E}_{S}[\ell(A_S,\mathbf{z})] &=\int \ell\big(A(\mathbf{z}_1,...,\mathbf{z}_m),\mathbf{z}\big)p(\mathbf{z}_1,...,\mathbf{z}_m) d\mathbf{z}_1...d\mathbf{z}_m \\
&=\int \ell\big(A(\mathbf{z}_1,...,\mathbf{z}_m),\mathbf{z}\big)p(\mathbf{z}_1)...p(\mathbf{z}_m)  d\mathbf{z}_1...d\mathbf{z}_m \\
\end{split}
\end{equation}$$

$$\begin{equation}
\begin{split}
\mathbf{E}_{S}[\ell(A_S,\mathbf{z}_j)] &=\int \ell\big(A(\mathbf{z}_1,..,\mathbf{z}_j,..,\mathbf{z}_m),\mathbf{z}_j\big)p(\mathbf{z}_1,..,\mathbf{z}_j,..,\mathbf{z}_m) d\mathbf{z}_1...d\mathbf{z}_m \\
& = \int \ell\big(A(\mathbf{z}_1,..,\mathbf{z}_j,..,\mathbf{z}_m),\mathbf{z}_j\big)p(\mathbf{z}_1)..p(\mathbf{z}_j)..p(\mathbf{z}_m) d\mathbf{z}_1...d\mathbf{z}_m \\
& = \int \ell\big(A(\mathbf{z}_1,..,\mathbf{z}_i^{'},..,\mathbf{z}_m),\mathbf{z}_i^{'}\big)p(\mathbf{z}_1)..p(\mathbf{z}_i^{'})..p(\mathbf{z}_m) d\mathbf{z}_1..d\mathbf{z}_i^{'}..d\mathbf{z}_m \\
& = \int \ell\big(A(\mathbf{z}_1,..,\mathbf{z}_i^{'},..,\mathbf{z}_m),\mathbf{z}_i^{'}\big)p(\mathbf{z}_1,..,\mathbf{z}_i^{'},..,\mathbf{z}_m) d\mathbf{z}_1..d\mathbf{z}_i^{'}..d\mathbf{z}_m  \int p(\mathbf{z}_i) d\mathbf{z}_i\\
& = \int \ell\big(A(\mathbf{z}_1,..,\mathbf{z}_i^{'},..,\mathbf{z}_m),\mathbf{z}_i^{'}\big)p(\mathbf{z}_1,..,\mathbf{z}_i,\mathbf{z}_i^{'},..,\mathbf{z}_m) d\mathbf{z}_1...d\mathbf{z}_m d\mathbf{z}_i^{'}\\
&=\mathbf{E}_{S,z_{i}^{'}}[\ell(A_{S^{i}},\mathbf{z}_i^{'})]
\end{split}
\end{equation}$$



$$\begin{equation}
\begin{split}
\mathbf{E}_S[R(A_S)-R_{emp}(A_S)] &= \mathbf{E}_{S}[\mathbf{E}_z[\ell(A_S,\mathbf{z})]]-\frac{1}{m}\sum\limits_{j=1}^{m}\mathbf{E}_{S}[\ell(A_S,\mathbf{z}_j)] \\
&= \mathbf{E}_{S}[\mathbf{E}_z[\ell(A_S,\mathbf{z})]]-\mathbf{E}_{S}[\ell(A_S,\mathbf{z}_j)] \\
%& =\mathbf{E}_{S}[\mathbf{E}_{z_{i}^{'}}[\ell(A_S,\mathbf{z}_{i}^{'})]] -    \mathbf{E}_{z_{i}^{'}}[\mathbf{E}_{S}[\ell(A_{S^{i}},\mathbf{z}_i^{'})]] \\
& =\mathbf{E}_{S,z_{i}^{'}}[\ell(A_S,\mathbf{z}_{i}^{'})]]- \mathbf{E}_{S,z_{i}^{'}}[\ell(A_{S^{i}},\mathbf{z}_i^{'})]\\
& =\mathbf{E}_{S,z_{i}^{'}}[\ell(A_S,\mathbf{z}_{i}^{'})-\ell(A_{S^{i}},\mathbf{z}_{i}^{'})] \\
\end{split}
\end{equation}$$

$$\begin{equation}
\begin{split}
|R(A_S)-R(A_{S^{\backslash i}})|&=  |\mathbf{E}_z[\ell(A_S,\mathbf{z})]-\mathbf{E}_z[\ell(A_{S^{\backslash i}},\mathbf{z})]| \\
& =	|\mathbf{E}_z[\ell(A_S,\mathbf{z}) - \ell(A_{S^{\backslash i}},\mathbf{z})] |\\
& \leq \mathbf{E}_z[|\ell(A_S,\mathbf{z})-\ell(A_{S^{\backslash i}},\mathbf{z})|] \\
& \leq \mathbf{E}_z[\beta]=\beta \\
|R(A_{S^{i}})-R(A_{S^{\backslash i}})|& \leq  \mathbf{E}_z[|\ell(A_{S^{i}},\mathbf{z})-\ell(A_{S^{\backslash i}},\mathbf{z})|] \\
& \leq \mathbf{E}_z[\beta]=\beta \\
\end{split}
\end{equation}$$

<!---
$$\begin{equation}
\begin{split}
|R_{emp}(A_S)-R_{emp}(A_{S^{\backslash i}})|&= \Big|\frac{1}{m}\sum\limits_{j=1}^{m}\ell(A_S,\mathbf{z}_j)-\frac{1}{m}\sum\limits_{j=1,j \neq i}^{m}\ell(A_{S^{\backslash i}},\mathbf{z}_j) \Big|  \\
& \leq \Big| \frac{1}{m}\sum\limits_{j=1,j \neq i}^{m} (\ell(A_S,\mathbf{z}_j)-\ell(A_{S^{\backslash i}},\mathbf{z}_j)) \Big|+\Big| \frac{1}{m}\ell(A_S,\mathbf{z}_i) \Big| \\
& \leq \frac{(m-1)}{m}\beta  +\frac{M}{m} \\
& \leq \beta  +\frac{M}{m} \\
\end{split}
\end{equation}$$
-->

$$\begin{equation}
\begin{split}
|R(A_S)-R(A_{S^{i}})|& \leq|R(A_S)-R(A_{S^{\backslash i}})|+|R(A_{S^{\backslash i}}) - R(A_{S^{i}}) |\\
& \leq 2\beta \\
\end{split}
\end{equation}$$

$$\begin{equation}
\begin{split}
|R_{emp}(A_S)-R_{emp}(A_{S^{i}})| & \leq |\frac{1}{m}\sum\limits_{j=1,j \neq i}^{m} (\ell(A_S,\mathbf{z}_j)-\ell(A_{S^{i}},\mathbf{z}_j))|+|\frac{1}{m} (\ell(A_S,\mathbf{z}_i)-\ell(A_{S^{i}},\mathbf{z}_i^{'})) |\\
& \leq  |\frac{1}{m}\sum\limits_{j=1,j \neq i}^{m} (\ell(A_S,\mathbf{z}_j)-\ell(A_{S^{\backslash i}},\mathbf{z}_j))|+ |\frac{1}{m}\sum\limits_{j=1,j \neq i}^{m} (\ell(A_{S^{i}},\mathbf{z}_j)-\ell(A_{S^{\backslash i}},\mathbf{z}_j))|  + \\
& |\frac{1}{m} (\ell(A_S,\mathbf{z}_i)-\ell(A_{S^{i}},\mathbf{z}_i^{'})) |\\
& \leq  2\frac{(m-1)}{m}\beta +\frac{M}{m} \\
& \leq   2\beta +\frac{M}{m} \\
\end{split}
\end{equation}$$

>Note: $$(\ell(A_S,\mathbf{z}_i)-\ell(A_{S^{i}},\mathbf{z}_i^{'}))$$ introduces $$M$$ (bound on loss) in the equation. If your loss is **unbounded** then you may be able to bound $$(\ell(A_S,\mathbf{z}_i)-\ell(A_{S^{i}},\mathbf{z}_i^{'}))$$ and replace $$M$$ by it, in the generalization bound.

$$\begin{equation}
\begin{split}
|\big(R(A_S)-R_{emp}(A_{S})\big) -\big(R(A_{S^{i}})-R_{emp}(A_{S^{i}})\big)|& \leq |R(A_S)-R(A_{S^{i}})| + |R_{emp}(A_S)-R_{emp}(A_{S^{i}})|\\
&\leq 2\beta+2\beta+\frac{M}{m} \\
& =4\beta+\frac{M}{m} \\
\end{split}
\end{equation}$$


$$\begin{equation}
\begin{split}
\mathbf{E}_{S}[R(A_S)-R_{emp}(A_S)] &= \mathbf{E}_{S,z_{i}^{'}}[\ell(A_S,\mathbf{z}_{i}^{'})-\ell(A_{S^{i}},\mathbf{z}_{i}^{'})] \\
& \leq \mathbf{E}_{S,z_{i}^{'}}[|\ell(A_S,\mathbf{z}_{i}^{'})-\ell(A_{S^{i}},\mathbf{z}_{i}^{'})|]\\
& \leq \mathbf{E}_{S,z_{i}^{'}}[|\ell(A_S,\mathbf{z}_{i}^{'})-\ell(A_{S^{\backslash i}},\mathbf{z}_{i}^{'})| + |\ell(A_{S^{i}},\mathbf{z}_{i}^{'})-\ell(A_{S^{\backslash i}},\mathbf{z}_{i}^{'})|]\\
& \leq 2\beta \\
\end{split}
\end{equation}$$

>Applying McDiarmid's inequality to derive generalization bounds for uniform stable algorithms:
>
><center>$$ \mathbf{P}\Bigg( \Big(R(A_S)-R_{emp}(A_S)\Big)  - 2\beta \geq   \epsilon \Bigg)  \leq \underbrace{e^{-\frac{2\epsilon^2}{m(4\beta+\frac{M}{m})^2}}}_{\delta} $$
>$$ \implies    \mathbf{P}\Bigg( \Big(R(A_S)-R_{emp}(A_S)\Big) \leq  2\beta +  (4m\beta+M)\sqrt{\frac{\log \frac{1}{\delta}}{m}} \Bigg) \geq 1-\delta $$</center>
