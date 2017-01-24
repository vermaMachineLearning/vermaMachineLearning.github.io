---
title:  " Blog: Stability of Learning Algorithms Part 3."
date:   2017-01-18 00:02:00
categories: Tutorials
---


**Extending to Ranking Algorithms**


Let $$\mathbf{z}=(\mathbf{x},y)$$ be a sample such that $$y\in \mathbb{R}$$ represents the ranking score. If $$y_1>y_2$$, then $$\mathrm{rank}(\mathbf{x}_1)>\mathrm{rank}(\mathbf{x}_2)$$. Let $$A_{S}$$ be such a ranking score learning function.

We define loss function on a pairwise sample $$(\mathbf{z},\mathbf{z}^{'})$$ and compute a real-valued loss i.e., $$\ell:\mathbf{Z}^{m} \times \mathbf{Z}\times \mathbf{Z} \rightarrow \mathbb{R}$$. We also require our loss function to be symmetric on pairwise sample i.e, $$\ell(A_S,\mathbf{z},\mathbf{z}^{'})=\ell(A_S,\mathbf{z}^{'},\mathbf{z})$$ and bounded as follows, $$0\leq \ell(A_S,\mathbf{z},\mathbf{z}^{'}) \leq M$$ for ranking problem.

We define the generalization error or risk $$R(A_S)$$ as follows:

$$R(A_S):=\mathbf{E}_{z,z^{'}}[\ell(A_S,\mathbf{z},\mathbf{z}^{'})]=\int \ell(A_S,\mathbf{z},\mathbf{z}^{'}) p(\mathbf{z},\mathbf{z}^{'}) d\mathbf{z}d\mathbf{z}^{'}$$

Empirical error $$R_{emp}(A_S)$$ is defined as follows:

$$R_{emp}(A_S):=\frac{1}{\binom{m}{2}}\sum\limits_{i=1}^{m-1}\sum\limits_{j=i+1}^{m}\ell(A_S,\mathbf{z}_i,\mathbf{z}_j)$$


>Let's assume that our algorithm $$A_S$$ has uniform stability $$\beta$$, i.e.  it satisfies $$\forall i\in\{1,m\}$$,
>
>$$ \underset{S,z,z^{'}}{\sup}|\ell(A_S,\mathbf{z},\mathbf{z}^{'}) -\ell(A_{S^{i}},\mathbf{z},\mathbf{z}^{'})| \leq \beta $$

**Note**: The definition here, involves $$S^{i}$$ rather than $$S^{\backslash i}$$ which is slightly different than uniform stability definitions.


$$\begin{equation}
\begin{split}
|R(A_S)-R(A_{S^{i}})| & = \Big|\mathbf{E}_{z,z^{'}}[\ell(A_S,\mathbf{z},\mathbf{z}^{'})] - \mathbf{E}_{z,z^{'}}[\ell(A_{S^{i}},\mathbf{z},\mathbf{z}^{'})] \Big|  \\
&=\Big| \mathbf{E}_{z,z^{'}}[\ell(A_S,\mathbf{z},\mathbf{z}^{'})-\ell(A_{S^{i}},\mathbf{z},\mathbf{z}^{'})] \Big| \\
& \leq \beta \\
\end{split}
\end{equation}$$

$$\begin{equation}
\begin{split}
|R_{emp}(A_S)-R_{emp}(A_{S^{k}})| & =  \frac{1}{\binom{m}{2}}\sum\limits_{i=1,i\neq k}^{m-1}\sum\limits_{j=i+1,j\neq k}^{m}\Bigg|\Big(\ell(A_S,\mathbf{z}_i,\mathbf{z}_j) - \ell(A_{S^{k}},\mathbf{z}_i,\mathbf{z}_j) \Big) \Bigg| + \\
& \hspace{1.5em} \frac{1}{\binom{m}{2}}\sum\limits_{i=1,i \neq k}^{m}\Bigg|\Big(\ell(A_S,\mathbf{z}_i,\mathbf{z}_k) - \ell(A_{S^{k}},\mathbf{z}_i,\mathbf{z}_k) \Big) \Bigg|  \\
& \leq  \frac{1}{\binom{m}{2}}\Big(\binom{m}{2}-(m-1)\Big)\beta + (m-1) M \\
& < \beta+\frac{2M}{m}
\end{split}
\end{equation}$$

$$\begin{equation}
\begin{split}
|\Big(R(A_S)- R_{emp}(A_S)\Big)-\Big(R(A_{S^{i}})-R_{emp}(A_{S^{i}})\Big)| & \leq |R(A_S)-R(A_{S^{i}})|+|R_{emp}(A_S)-R_{emp}(A_{S^{i}})| \\
& \leq 2\beta+\frac{2M}{m} \\
\end{split}
\end{equation}$$

$$\begin{equation}
\begin{split}
\mathbf{E}_{S}[R(A_S)] &= \mathbf{E}_{S,z_{i}^{'},z_{j}^{'}}[\ell(A_{S},\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'}) \\
\mathbf{E}_{S}[R_{emp}(A_S)] &= \mathbf{E}_{S,z_{i}^{'},z_{j}^{'}}[\ell(A_{S^{i,j}},\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'})] \\
\end{split}
\end{equation}$$

$$\begin{equation}
\begin{split}
\mathbf{E}_{S}[R(A_S)-R_{emp}(A_S)] &= \mathbf{E}_{S,z_{i}^{'},z_{j}^{'}}[\ell(A_S,\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'})-\ell(A_{S^{i,j}},\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'})] \\
& \leq \mathbf{E}_{S,z_{i}^{'},z_{j}^{'}}[|\ell(A_S,\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'})-\ell(A_{S^{i}},\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'}) + \\
& \hspace{1.5em} \ell(A_{S^{i}},\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'})   -\ell(A_{S^{i,j}},\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'}) |] \\
& \leq \mathbf{E}_{S,z_{i}^{'},z_{j}^{'}}[|\ell(A_S,\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'})-\ell(A_{S^{i}},\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'})|] +\\
& \hspace{1.5em} \mathbf{E}_{S,z_{i}^{'},z_{j}^{'}}[|\ell(A_{S^{i}},\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'})   -\ell(A_{S^{i,j}},\mathbf{z}_{i}^{'},\mathbf{z}_{j}^{'}) |] \\
& \leq 2\beta \\
\end{split}
\end{equation}$$

>Applying McDiarmid's inequality to derive generalization bounds for uniform stable algorithms:
>
><center>$$ \mathbf{P}\Bigg( \Big(R(A_S)-R_{emp}(A_S)\Big)  - 2\beta \geq   \epsilon \Bigg)  \leq \underbrace{e^{-\frac{2\epsilon^2}{m(2\beta+\frac{2M}{m})^2}}}_{\delta} $$
>$$ \implies    \mathbf{P}\Bigg( \Big(R(A_S)-R_{emp}(A_S)\Big) \leq  2\beta +  (m\beta+M)\sqrt{\frac{2\log \frac{1}{\delta}}{m}} \Bigg) \geq 1-\delta $$</center>
