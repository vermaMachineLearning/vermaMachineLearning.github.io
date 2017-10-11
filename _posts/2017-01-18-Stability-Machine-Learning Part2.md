---
title:  " Blog: Stability of Learning Algorithms Part 2."
date:   2017-01-18 00:01:30
categories: Tutorials
visible: 1
---

**Extending to Randomized Algorithms**

Let $$R$$ be  a set of random parameters.

Let's assume that our algorithm $$A_{(S,R)}$$ has uniform stability $$(\beta,\rho)$$, if  it satisfies,

$$ \underset{S,z}{\sup}|\mathbf{E}_{R}[\ell(A_{(S,R)},\mathbf{z})] -\mathbf{E}_{R}[\ell(A_{(S^{\backslash i},R)},\mathbf{z})]| \leq \beta \hspace{3em} \forall i\in\{1,m\}$$

$$\underset{S,R,r_i^{'},z}{\sup}| \ell(A_{(S,R)},\mathbf{z}) -\ell(A_{(S,R^{i})},\mathbf{z})| \leq \rho \hspace{3em} \forall i\in\{1,T\}$$

<!---
>$$\underset{r_1,...,r_T,r_i^{'},z}{\sup}| \ell(A_{S,(r_1,...,r_T)},\mathbf{z}) -\ell(A_{S,(r_1,..,r_{i-1},r_i^{'},r_{i+1},..,r_T)},\mathbf{z})| \leq \rho $$
-->


$$K_{(S,R)}=R(A_{(S,R)})-R_{emp}(A_{(S,R)})$$

$$\begin{equation}
\begin{split}
|K_{(S,R)}-K_{(S,R^{i})}|&=\Big| \big(R(A_{(S,R)})-R_{emp}(A_{(S,R)}) \big) - \big( R(A_{(S,R^{i})}) - R_{emp}(A_{(S,R^{i})})\big) \Big|\\
&=  \Big| \mathbf{E}_z [\ell(A_{(S,R)},\mathbf{z})- \ell(A_{(S,R^{i})},\mathbf{z})]-\frac{1}{m}\sum\limits_{i=1}^{m} (\ell(A_{(S,R)},\mathbf{z}_i)- \ell(A_{(S,R^{i})},\mathbf{z}_i))     \Big|\\
&\leq \mathbf{E}_z [|\ell(A_{(S,R)},\mathbf{z})- \ell(A_{(S,R^{i})},\mathbf{z})|]+\frac{1}{m} \sum\limits_{i=1}^{m}|(\ell(A_{(S,R)},\mathbf{z}_i)- \ell(A_{(S,R^{i})},\mathbf{z}_i))| \\
& \leq 2\rho \\
\end{split}
\end{equation}$$


Applying McDiarmid's inequality assuming $$S$$ is fixed, we get (Event 1),

$$P\Bigg(  K_{(S,R)}-\mathbf{E}_R[K_{(S,R)}] \geq \epsilon \Big|  S  \Bigg) \leq \underbrace{e^{-\frac{\epsilon^2}{2T\rho^2}}}_{\delta}$$

$$P\Bigg(  K_{(S,R)}-\mathbf{E}_R[K_{(S,R)}] \leq  \rho\sqrt{2T} \sqrt{\log \big(\frac{1}{\delta} \big)}  \Big|   S  \Bigg) \geq 1-\delta$$


$$\begin{equation}
\begin{split}
\mathbf{E}_{S,R}[K_{(S,R)}] &= \mathbf{E}_{S,R}[R(A_{(S,R)})] - \mathbf{E}_{S,R}[R_{emp}(A_{(S,R)})]  \\
&= \mathbf{E}_{S,R}[\mathbf{E}_z[\ell(A_{(S,R)},\mathbf{z})]] - \mathbf{E}_{S,R}[\frac{1}{m}\sum\limits_{j=1}^{m}\ell(A_{(S,R)},\mathbf{z}_j)]  \\
& = \mathbf{E}_{S,z}[\mathbf{E}_R[\ell(A_{(S,R)},\mathbf{z})]] - \mathbf{E}_{S} [\mathbf{E}_R[\ell(A_{(S,R)},\mathbf{z}_j)]]\\
& = \mathbf{E}_{S,z}[\mathbf{E}_R[\ell(A_{(S,R)},\mathbf{z})]]- \mathbf{E}_{S,z}[\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z})]] + \mathbf{E}_{S,z}[\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z})]] - \\
& \hspace{3em} \mathbf{E}_{S} [\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z}_j)]]+\mathbf{E}_{S} [\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z}_j)]] - \mathbf{E}_{S} [\mathbf{E}_R[\ell(A_{(S,R)},\mathbf{z}_j)]]\\
& = \mathbf{E}_{S,z}[\mathbf{E}_R[\ell(A_{(S,R)},\mathbf{z})] -  \mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z})]] + \mathbf{E}_{S,z}[\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z})]] -\\
& \hspace{3em} \mathbf{E}_{S} [\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z}_j)]] + \mathbf{E}_{S} [\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z}_j)]-\mathbf{E}_R[\ell(A_{(S,R)},\mathbf{z}_j)]]\\
& \leq 2\beta + \mathbf{E}_{S,z}[\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z})]] - \mathbf{E}_{S} [\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z}_j)]]\\
& \leq 2\beta + \mathbf{E}_{S,z_j}[\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z}_j)]] - \mathbf{E}_{S} [\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z}_j)]]\\
& \leq 2\beta + \mathbf{E}_{S}[\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z}_j)]] - \mathbf{E}_{S} [\mathbf{E}_R[\ell(A_{(S^{\backslash i},R)},\mathbf{z}_j)]]\\
& = 2\beta \\
\end{split}
\end{equation}$$



<!---

$$\begin{equation}
\begin{split}
\underset{S}{\sup} \Big|\mathbf{E}_R[K_{(S,R)}]-\mathbf{E}_R[K_{(S^{i},R)}]\Big|&=\underset{S}{\sup}\Big| \mathbf{E}_R[K_{(S,R)}]-\mathbf{E}_R[K_{(S^{\backslash i},R)}] + \\
& \hspace{2em} \mathbf{E}_R[K_{(S^{\backslash i},R)}] -\mathbf{E}_R[K_{(S^{i},R)}]  \Big|  \\
\end{split}
\end{equation}$$

$$\begin{equation}
\begin{split}
& = \underset{S}{\sup}\Big|  \Big|\\
\end{split}
\end{equation}$$


--->



$$\begin{equation}
\begin{split}
|\mathbf{E}_R[K_{(S,R)}]-\mathbf{E}_R[K_{(S^{i},R)}]|&=\Big| \mathbf{E}_R[\big(R(A_{(S,R)})-R_{emp}(A_{(S,R)}) \big)] - \mathbf{E}_R[\big( R(A_{(S^{i},R)}) - R_{emp}(A_{(S^{i},R)})\big)] \Big|\\
& \leq \Big| \mathbf{E}_R[R(A_{(S,R)})]-\mathbf{E}_R[R(A_{(S^{i},R)})] \Big| +\Big| \mathbf{E}_R[R_{emp}(A_{(S,R)})]-\mathbf{E}_R[R_{emp}(A_{(S^{i},R)})] \Big|\\
& \leq 2\beta+(2\beta+\frac{M}{m}) \\
& \leq 4\beta+\frac{M}{m} \\
\end{split}
\end{equation}$$

Applying McDiarmid's inequality, we get (Event 2),


$$P \Bigg(  \mathbf{E}_R[K_{(S,R)}]-\mathbf{E}_S[\mathbf{E}_R[K_{(S,R)}]] \geq \epsilon   \Bigg) \leq \underbrace{e^{-\frac{2\epsilon^2}{m(4\beta+\frac{M}{m})^2}}}_{\delta}$$

$$P \Bigg(  \mathbf{E}_R[K_{(S,R)}] \leq  2\beta +  (4m\beta+M)\sqrt{\frac{\log \frac{1}{\delta}}{2m}} \Bigg) \geq 1-\delta$$



Now, probability of holding both the events simultaneously is $$1-2\delta$$.


$$\mathbf{P}\Bigg( R(A_{(S,R)})-R_{emp}(A_{(S,R)})  \leq  2\beta +  (4m\beta+M)\sqrt{\frac{\log \frac{1}{\delta}}{2m}} +  \rho\sqrt{2T} \sqrt{\log \big(\frac{1}{\delta} \big)} \Bigg) \geq 1-2\delta $$

Replacing $$\delta$$ by $$\frac{\delta}{2}$$, we get,

>$$\mathbf{P}\Bigg( \ R(A_{(S,R)})-R_{emp}(A_{(S,R)})  \leq  2\beta +  \Big( \frac{4m\beta+M}{\sqrt{2m}} +  \rho\sqrt{2T} \Big) \sqrt{\log \big(\frac{2}{\delta} \big) }   \Bigg) \geq 1-\delta $$

