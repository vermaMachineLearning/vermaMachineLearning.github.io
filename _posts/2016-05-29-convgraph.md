---
title:  "Blog: Learning Convolutional Networks for Graphs"
date:   2016-05-29 22:37:00
categories: PaperList
---
For past couple of months, I have been wondering about how convolutional networks can be applied to  graphs. As we know, convolutional networks have became the state of the art in image classification and also in natural language processing. It is easy to see how convolutional networks  exploits the locality and translation invariance properties in an image. People have now also realized on how to exploit those properties in NLP using convolutional networks efficiently. It is time to look at other, more general, domains such as  *graphs* where notion of locality or receptive field needs to be defined. At this point, we can start thinking about graph neighborhood concepts as it is going to play a major role in connecting with receptive fields of convolutional networks.

[//]: # (There are only handful of papers which  tries to crack this problem. One of them is: *Deep Convolutional Networks on Graph-Structured Data* [[PDF2015]](http://arxiv.org/pdf/1506.05163v1.pdf). But, I am)


There are two problem formulation exist in literature:


>1. Given a set of graphs $$S_G=\{G_1,G_2,...,G_n\}$$ and labels $$Y=\{y_1,y_2,...,y_n\}\in \mathbf{R}$$, learn a function $$f:G\rightarrow\mathbf{R}$$ such that it maps graphs to appropriate labels using convolutional networks.
2. Generalization of convolutional network to a single graph $$G$$ where each input example is a vertex.

The first problem is somewhat becomes usual machine learning problem, if we know how to construct/extract features from a graph and then we can simply train any machine learning algorithm. Second problem, is different in the sense that each node itself is a data point in a given graph, so it is not apparent what could be a potential feature vector here. Personally, I am more interested in second the one.

Without diving into previous works (there are many), I'll layout the findings of:  *Learning Convolutional Neural Networks for Graphs* ICML2016 paper which deal with the first problem formulation. The idea is to bring an order among nodes in a graph. So authors introduce the notion of graph labeling which decide the rank of nodes. Following steps are involved in their overall approach:

>1. First compute a graph labeling to define rank on the nodes. For example, if $$l(u) > l(v)$$, then $$r(u)>r(v)$$ where $$l(u)$$ and $$r(u)$$ is the label & rank of node $$u$$ respectively. This labeling can be done based on degree of node, page-rank etc.
2. Next sort the vertices/nodes of the input graph with respect to a given graph labeling. Then the resulting node sequence, say $$NS$$ which is 1-dimensional, is traversed using a given stride $$s$$.
3. For each visited node (through stride $$s$$) in $$NS$$, construct a receptive field, until exactly $$w$$ receptive fields have been created. (Wait. What is receptive field here ?)
4. Receptive field is constructed based on the neighborhood of a node. This neighborhood can be defined based on adjacent nodes or shortest distance between nodes. Given   a node $$v$$ and the size of the receptive field $$k$$, the procedure performs a breadth-first search to find exactly $$k$$ neighbor nodes. Special care is taken when there are less  or more than $$k$$ (in case of tie) neighbor nodes. We can call this neighborhood of a node as sub-graph neighborhood.
5. On each sub-graph neighborhood, final and crucial step is performed called graph normalization and canonicalization.

To understand what graph canonicalization is, you need to understand what is graph isomorphism. Loosely speaking, if two graphs say $$G_1$$ & $$G_2$$ have different vertex labels and edge labels but graph $$G_1$$ can be re-labeled such that adjacency matrix becomes equal to graph $$G_2$$, then $$G_1$$ is isomorphic to $$G_2$$. In other words they are the same (in structure), its just their labellings are different. So, how do we know if two graphs are isomorphic or not. If somehow, we can represent each graph in some form, say a string of binary numbers, then we can just check if the binary representation of two graphs is same or not. This will tell us whether graphs are isomorphic or not. Of-course, this representation needs to hold some properties for such type of checking. This representation through which we can determine isomorphism is called canonical representation of a graph or $$canon(G)$$.  Basically, in canonical representation you can compare the two graphs just as you can compare the two images of cat. Canonicalization is a NP-hard problem, so necessary optimization is required.
Finally:

>Each canonical representation of sub-graph neighborhood  becomes the receptive field  and   combined with the normal convolutional networks. The vertex (and edge) attributes becomes the channel (like three RGB channels in an image).


So, magic lies in canonicalization of a graph for which author resorts to something called *`Nauty`* software, which performs canonicalization. I don't know the details of the canonicalization algorithm but it does take degree of a node into account.

This is the high-level approach authors adopt and experimentally it outperform the classification accuracy obtained by graph kernel algorithms for many datasets (eg. protein structure and chemical compounds related).

**References:**

1. Niepert, Mathias, Mohamed Ahmed, and Konstantin Kutzkov. "Learning Convolutional Neural Networks for Graphs." ICML 2016. [[PDF]](https://arxiv.org/pdf/1605.05273v2.pdf)
2. Henaff, Mikael, Joan Bruna, and Yann LeCun. "Deep convolutional networks on graph-structured data." arXiv preprint arXiv:1506.05163 (2015).[[PDF]](http://arxiv.org/pdf/1506.05163v1.pdf)
3. Bruna, Joan, et al. "Spectral networks and locally connected networks on graphs." arXiv preprint arXiv:1312.6203 (2013). [[PDF]](https://arxiv.org/pdf/1312.6203.pdf)
4. Coates, Adam, and Andrew Y. Ng. "Selecting receptive fields in deep networks." Advances in Neural Information Processing Systems. 2011. [[PDF]](http://robotics.stanford.edu/~ang/papers/nips11-SelectingReceptiveFields.pdf)
5. Yanardag, Pinar, and S. V. N. Vishwanathan. "Deep graph kernels." Proceedings of the 21th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining. ACM, 2015.[[PDF]](http://web.ics.purdue.edu/~ypinar/kdd/deep_graph_kernels.pdf)
