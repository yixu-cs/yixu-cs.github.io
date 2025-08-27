---
title: "Sublinear Algorithms for Estimating Single-Linkage Clustering Costs"
collection: publications
category: submitted
permalink: /publication/2025-SODA-SLC-costs
excerpt: 'We proposed new cost functions about SLC, which reflects properties of datasets, and estimated our costs in sublinear time. We studied this problem in both distance and similarity measurements, and conducted extensive experiments to validate our results. We also proved the lower bounds, and our algorithms nearly match them.'
date: 2025-07-14
venue: 'SODA 2026'
slidesurl: 
paperurl: 
bibtexurl: 
authors: Pan Peng, Christian Sohler, <b>Yi Xu</b> (alphabetical order)
---

Authors: Pan Peng, Christian Sohler, <b>Yi Xu</b> (alphabetical order)

Abstract
------
Single-linkage clustering is a fundamental method for data analysis. 
A single-linkage clustering is obtained by iteratively merging the closest pair of clusters, where initially every data point forms its own cluster and the distance between clusters is defined by their closest pair of points. Algorithmically, one can compute a single linkage $$k$$-clustering (a partition into $$k$$ clusters) by computing a minimum spanning tree and dropping the $$k-1$$ most costly edges. 
This clustering minimizes the sum of spanning tree weights of the clusters. This motivates us to define the cost $$\mathrm{cost}_k$$ of a single linkage $$k$$-clustering by the weight of the corresponding spanning forest. Besides, if we consider single linkage clustering as computing a hierarchy of clusterings, the total cost of the hierarchy is defined as the sum of the individual clusterings. 

In this paper,
we assume that the distances between data points are given as a graph $$G$$ with average degree $$d$$ and edge weights from $$\{1,\dots, W\}$$. If there is no edge, we assume the distance to be infinite.
We give a sampling based algorithm that is given access to the adjacency list of $$G$$ and that computes in $$\tilde O(d\sqrt{W}/\varepsilon^3)$$ time a succinct representation of estimates $$\widehat{\mathrm{cost}}_k$$ of the cost of every $$k$$-clustering such that $$\sum_{k=1}^n |\widehat{\mathrm{cost}}_k - \mathrm{cost}_k| \le \varepsilon\cdot \mathrm{cost}(G)$$, where $$\mathrm{cost}(G) = \sum_{k=1}^n \mathrm{cost}_k$$ is the total cost of the hierarchical clustering and $$0<\varepsilon <1$$. Thus we can approximate the cost of every $$k$$-clustering upto an absolute error that *on average* constitutes a $$(1+\varepsilon)$$-approximation of the true cost.
In particular, our result ensures that we can estimate $$\mathrm{cost}(G)$$ upto a factor of $$1\pm \varepsilon$$ in the same running time. We further prove that our algorithm is nearly optimal by establishing a lower bound of $$\Omega(d\sqrt{W}/\varepsilon^2)$$ on the query complexity required to estimate $$\mathrm{cost}(G)$$. We extend our results to the similarity setting, where edges represent similarities rather than distances and clusterings are defined by a maximum spanning tree. In this case, our algorithms run in time $$\tilde{O}(dW/\varepsilon^3)$$.
We also prove a nearly matching lower bound of $$\Omega(Wd/\varepsilon^2)$$ queries for estimating the total similarity-based cost.
This establishes an interesting -- and perhaps surprising -- separation between the distance and similarity settings. Finally, we extend our algorithms to metric space settings and validate our theoretical findings through extensive experiments.

My contribution
------

I refined the algorithm’s details and was primarily responsible for formulating proofs of key theorems and validating results through comprehensive experiments.