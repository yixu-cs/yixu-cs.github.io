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

Motivation
------

A single-linkage clustering (SLC) is obtained by iteratively merging the **closest** pair of clusters, where initially every data point forms its own cluster. Given a weighted graph, construction the SLC hierarchy requires linear time and accessing the whole input, which is not acceptable in big data era. Therefore, our goal is to **design informative cost functions to reflect properties of datasets, and then estimate these costs efficiently in sublinear time**. This allows us to summarize the clustering structure of data without accessing the whole input.

<p align="center">
  <img src="/images/paper-SLC/SLC.png"/>
</p>

Algorithmically, one can compute a single linkage $$k$$-clustering (a partition into $$k$$ clusters) by computing a minimum spanning tree and dropping the $$k-1$$ most costly edges. 

This clustering minimizes the sum of spanning tree weights of the clusters. This motivates us to define the cost $$\mathrm{cost}_k$$ of a single linkage $$k$$-clustering by the weight of the corresponding spanning forest. Besides, if we consider single linkage clustering as computing a hierarchy of clusterings, the total cost of the hierarchy is defined as the sum of the individual clusterings. That is, $$\mathrm{cost}(G) = \sum_{k=1}^n \mathrm{cost}_k$$.

The naive method is to compute a minimum spanning tree first, then compute exact values of $$\mathrm{cost}_k$$ and $$\mathrm{cost}(G)$$, which is linear in time.

Our results
------

Let $$W$$ be the maximum edge weight in the graph, and $$d$$ be the average degree. Suppose the input graph is given in ajacency list model, and for any vertex, one can query its neighbor. We give sublinear algorithms to estimate the cost functions, almost matching the lower bound.

| $$(1+\varepsilon)$$-estimate of $$\mathrm{cost}(G)$$ | Succinct representation of $$\mathrm{cost}_k$$ | Lower bound         |
|--------------------|------------------|---------------------|
| $$\tilde{O}(\sqrt{W}/\varepsilon^3 d)$$ | $$\tilde{O}(\sqrt{W}/\varepsilon^3 d)$$ | $$\Omega(\sqrt{W}/\varepsilon^2 d)$$ |

**Succinct representation** meaning:  
For all $$k$$, we can recover $$\widehat{\mathrm{cost}_k}$$ in a short time, with average $$(1+\varepsilon)$$ error, which means:

$$
\sum_{k=1}^n |\widehat{\mathrm{cost}_k} - \mathrm{cost}_k| \le \varepsilon \cdot \mathrm{cost}(G),
$$

and query time for retrieving each $$k$$ is $$O(\log\log W)$$.

Techniques
------

1. **Reduction:** SLC cost estimation reduces to estimating the number of connected components (\#CCs) in thresholded subgraphs.
2. **Estimating connected components:**  
   - Use sampling and BFS ([\[CRT05\]](https://www.cs.princeton.edu/~chazelle/pubs/mstapprox.pdf)).
   - Our algorithm improves on prior bounds.
3. **Binary search for acceleration:**  
   - Since each \#CCs lies in the range $$[1,n]$$, we partition this range into intervals.

    <p align="center">
      <img src="/images/paper-SLC/map_intervals1.png" width="400" />
    </p>
    
   - We then map \#CCs to the nearest interval endpoints:  
    
    <p align="center">
      <img src="/images/paper-SLC/map_intervals2.png" width="400" />
    </p>

   - We choose endpoints wisely, such that the gap of two endpoints are close to the error bound of CC values.

Extension: similarity case
------

We further extend our results to another measurement: every edge weight in graph represents **similarity**, and in every step of SLC, it merges two most **similar** clusters.

We note that this measurement is more subtle and challenging to handle, than the distance case.

We obtain a similar result, but the query complexity is linear in $$W$$ now (in distance case, it's linear in $$\sqrt{W}$$).

| $$(1+\varepsilon)$$-estimate of $$\mathrm{cost}(G)$$ | Succinct representation of $$\mathrm{cost}_k$$ | Lower bound         |
|--------------------|------------------|-----------------------|
| $$\tilde{O}(W/\varepsilon^3 d)$$ | $$\tilde{O}(W/\varepsilon^3 d)$$ | $$\Omega(W/\varepsilon^2 d)$$ |

Experiments
------

- $$r$$: sample size for CC estimation; $$\widehat{\mathrm{cost}_k}$$: estimated value for $$\mathrm{cost}_k$$.


Approximation ratio         |  Speed up for datasets ($$r=100$$) | Speed up for various $$r$$
:-------------------------:|:-------------------------:|:-------------------------:
<img src='/images/paper-SLC/r_ratio_error_bar_combined.png' width='300' />  |  <img src='/images/paper-SLC/r_speed_up_combined_dist_log10.png' width='300' /> | <img src='/images/paper-SLC/r_list_combined_speed_up_italy.png' width='300' />

The sequence $$\{\mathrm{cost}_1, \mathrm{cost}_2, \ldots, \mathrm{cost}_n\}$$ can be regarded as a profile vector representing the SLC clustering hierarchy. Our experiments demonstrate that these profile vectors can be estimated with high accuracy for each dataset, effectively capturing the intrinsic structural properties of the data.

Profile in distance case         |  Profile in similarity case 
:-------------------------:|:-------------------------:
<img src='/images/paper-SLC/estimated_combined_profile_distance.png' width='300' />  |  <img src='/images/paper-SLC/estimated_combined_profile_similarity.png' width='300' /> 