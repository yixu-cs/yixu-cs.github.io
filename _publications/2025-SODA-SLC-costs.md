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

<!-- Abstract
------
Single-linkage clustering is a fundamental method for data analysis. 

In this paper,
we assume that the distances between data points are given as a graph $$G$$ with average degree $$d$$ and edge weights from $$\{1,\dots, W\}$$. If there is no edge, we assume the distance to be infinite.
We give a sampling based algorithm that is given access to the adjacency list of $$G$$ and that computes in $$\tilde O(d\sqrt{W}/\varepsilon^3)$$ time a succinct representation of estimates $$\widehat{\mathrm{cost}}_k$$ of the cost of every $$k$$-clustering such that $$\sum_{k=1}^n |\widehat{\mathrm{cost}}_k - \mathrm{cost}_k| \le \varepsilon\cdot \mathrm{cost}(G)$$, where $$\mathrm{cost}(G) = \sum_{k=1}^n \mathrm{cost}_k$$ is the total cost of the hierarchical clustering and $$0<\varepsilon <1$$. Thus we can approximate the cost of every $$k$$-clustering upto an absolute error that *on average* constitutes a $$(1+\varepsilon)$$-approximation of the true cost.
In particular, our result ensures that we can estimate $$\mathrm{cost}(G)$$ upto a factor of $$1\pm \varepsilon$$ in the same running time. We further prove that our algorithm is nearly optimal by establishing a lower bound of $$\Omega(d\sqrt{W}/\varepsilon^2)$$ on the query complexity required to estimate $$\mathrm{cost}(G)$$. We extend our results to the similarity setting, where edges represent similarities rather than distances and clusterings are defined by a maximum spanning tree. In this case, our algorithms run in time $$\tilde{O}(dW/\varepsilon^3)$$.
We also prove a nearly matching lower bound of $$\Omega(Wd/\varepsilon^2)$$ queries for estimating the total similarity-based cost.
This establishes an interesting -- and perhaps surprising -- separation between the distance and similarity settings. Finally, we extend our algorithms to metric space settings and validate our theoretical findings through extensive experiments. -->

Our goal
------

A single-linkage clustering is obtained by iteratively merging the closest pair of clusters, where initially every data point forms its own cluster and the distance between clusters is defined by their closest pair of points.

<p align="center">
  <img src="/images/paper-SLC/SLC.png" width="400"/>
</p>

Algorithmically, one can compute a single linkage $$k$$-clustering (a partition into $$k$$ clusters) by computing a minimum spanning tree and dropping the $$k-1$$ most costly edges. 

This clustering minimizes the sum of spanning tree weights of the clusters. This motivates us to define the cost $$\mathrm{cost}_k$$ of a single linkage $$k$$-clustering by the weight of the corresponding spanning forest. Besides, if we consider single linkage clustering as computing a hierarchy of clusterings, the total cost of the hierarchy is defined as the sum of the individual clusterings. 

We further define tha $$\mathrm{cost}(G) = \sum_{k=1}^n \mathrm{cost}_k$$ is the total cost of the hierarchical clustering.

Since computing a SLC structure requires linear time, our goal is to **estimate the cost functions $$\mathrm{cost}_k$$ and $$\mathrm{cost}(G)$$ in sublinear time**.

Our results
------

Let $$W$$ be the maximum edge weight in the graph, and $$d$$ be the average degree.

| $$\mathrm{cost}(G)$$ | $$\mathrm{cost}_k$$ | Lower Bound         |
|--------------------|------------------|---------------------|
| $$\tilde{O}(\sqrt{W}/\varepsilon^3 d)$$ | $$\tilde{O}(\sqrt{W}/\varepsilon^3 d)$$ | $$\Omega(\sqrt{W}/\varepsilon^2 d)$$ |

**Succinct Representation:**  
For all $$k$$, we can recover $$\widehat{\mathrm{cost}_k}$$ in short time, with average $$(1+\varepsilon)$$ error:

$$
\sum_{k=1}^n |\widehat{\mathrm{cost}_k} - \mathrm{cost}_k| \le \varepsilon \cdot \mathrm{cost}(G)
$$

Query time for retrieving each $$k$$: $$O(\log\log W)$$.

Proof Sketch
------

1. **Reduction:** SLC cost estimation reduces to estimating the number of connected components (CCs) in thresholded subgraphs.
2. **Estimating connected components:**  
   - Use sampling and BFS ([CRT05]).
   - Our algorithm improves on prior bounds.
3. **Binary search for acceleration:**  
   - Partition interval $$[1,n]$$ for CCs.
   - Map CC counts to nearest interval endpoints:  
    ![](/images/paper-SLC/map_intervals1.png)
    
    ![](/images/paper-SLC/map_intervals2.png)
   - Choice of endpoints illustrated: ![](/images/paper-SLC/endpoints.png)


Extension: Similarity Case
------

We further extend our results to another measurement: every edge weight in graph represents similarity, and in every step of SLC, it merges two most similar clusters.

We note that this measurement is more subtle and challenging to handle, than the distance case.

We obtain a similar result, but the query complexity is linear in $$W$$ now (in distance case, it's linear in $$\sqrt{W}$$).

| $$\mathrm{cost}(G)$$ | $$\mathrm{cost}_k$$ | Lower Bound           |
|--------------------|------------------|-----------------------|
| $$\tilde{O}(W/\varepsilon^3 d)$$ | $$\tilde{O}(W/\varepsilon^3 d)$$ | $$\Omega(W/\varepsilon^2 d)$$ |

Experiments
------

- $$r$$: sample size for CC estimation; $$\widehat{\mathrm{cost}_k}$$: estimated value for $$\mathrm{cost}_k$$.


Approximation ratio         |  Speed up among various datasets ($$r=$$) | Speed up for the same dataset
:-------------------------:|:-------------------------:|:-------------------------:
<img src='/images/paper-SLC/r_ratio_error_bar_combined.png' width='300' />  |  <img src='/images/paper-SLC/r_speed_up_combined_dist_log10.png' width='300' /> | <img src='/images/paper-SLC/r_list_combined_speed_up_italy.png' width='300' />

Profile in distance case         |  Profile in similarity case 
:-------------------------:|:-------------------------:
<img src='/images/paper-SLC/estimated_combined_profile_distance.png' width='300' />  |  <img src='/images/paper-SLC/estimated_combined_profile_similarity.png' width='300' /> 