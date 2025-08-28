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
A single-linkage clustering is obtained by iteratively merging the closest pair of clusters, where initially every data point forms its own cluster and the distance between clusters is defined by their closest pair of points. Algorithmically, one can compute a single linkage $$k$$-clustering (a partition into $$k$$ clusters) by computing a minimum spanning tree and dropping the $$k-1$$ most costly edges. 
This clustering minimizes the sum of spanning tree weights of the clusters. This motivates us to define the cost $$\mathrm{cost}_k$$ of a single linkage $$k$$-clustering by the weight of the corresponding spanning forest. Besides, if we consider single linkage clustering as computing a hierarchy of clusterings, the total cost of the hierarchy is defined as the sum of the individual clusterings. 

In this paper,
we assume that the distances between data points are given as a graph $$G$$ with average degree $$d$$ and edge weights from $$\{1,\dots, W\}$$. If there is no edge, we assume the distance to be infinite.
We give a sampling based algorithm that is given access to the adjacency list of $$G$$ and that computes in $$\tilde O(d\sqrt{W}/\varepsilon^3)$$ time a succinct representation of estimates $$\widehat{\mathrm{cost}}_k$$ of the cost of every $$k$$-clustering such that $$\sum_{k=1}^n |\widehat{\mathrm{cost}}_k - \mathrm{cost}_k| \le \varepsilon\cdot \mathrm{cost}(G)$$, where $$\mathrm{cost}(G) = \sum_{k=1}^n \mathrm{cost}_k$$ is the total cost of the hierarchical clustering and $$0<\varepsilon <1$$. Thus we can approximate the cost of every $$k$$-clustering upto an absolute error that *on average* constitutes a $$(1+\varepsilon)$$-approximation of the true cost.
In particular, our result ensures that we can estimate $$\mathrm{cost}(G)$$ upto a factor of $$1\pm \varepsilon$$ in the same running time. We further prove that our algorithm is nearly optimal by establishing a lower bound of $$\Omega(d\sqrt{W}/\varepsilon^2)$$ on the query complexity required to estimate $$\mathrm{cost}(G)$$. We extend our results to the similarity setting, where edges represent similarities rather than distances and clusterings are defined by a maximum spanning tree. In this case, our algorithms run in time $$\tilde{O}(dW/\varepsilon^3)$$.
We also prove a nearly matching lower bound of $$\Omega(Wd/\varepsilon^2)$$ queries for estimating the total similarity-based cost.
This establishes an interesting -- and perhaps surprising -- separation between the distance and similarity settings. Finally, we extend our algorithms to metric space settings and validate our theoretical findings through extensive experiments. -->


## Background

### Hierarchical Clustering

- Hierarchical clustering builds a hierarchy of clusters greedily.
- **Agglomerative (bottom-up)**: merge the closest clusters at each step.
- **Divisive (top-down)**: recursively split clusters.

**Illustrations:**  
Agglomerative: ![Agglomerative](/images/paper-SLC/hierarchical_bottom_up.png)  
Divisive: ![Divisive](/images/paper-SLC/hierarchical_top-down.png)

**Linkage Matrix:**  
![Linkage](/images/paper-SLC/hierarchical_linkage.png)

#### Applications

- **Community Detection:**  
  - Community: ![Community](/images/paper-SLC/hierarchcal_application_community1.png)  
  - Hierarchical tree: ![Hierarchical tree](/images/paper-SLC/hierarchcal_application_community2.png)  
  - [For10], Citations: 13112
- **Biology:**  
  - Human tumor: ![Human tumor](/images/paper-SLC/hierarchcal_application2.png)  
  - Tumor (different linkages): ![Tumor](/images/paper-SLC/hierarchcal_application1.png)  
  - DNA: ![DNA](/images/paper-SLC/hierarchcal_application3.png)  
  - [HTFF09], Elements of Statistical Learning
- **Other Fields:**  
  - Iris clusters: ![Iris](/images/paper-SLC/hierarchical_Iris.png)  
  - Software suite: ![Software suite](/images/paper-SLC/hierarchical_suite.png)

---

## Single Linkage Clustering (SLC)

- **Input:** Connected, weighted graph $G=(V,E)$ representing distances.
- **Method:** Iteratively merge the two closest clusters.
- **Example:** $V=\{a,b,c,d,e\}$, with step-by-step clustering:
    - ![](/images/paper-SLC/SLC0.png)  
    - ![](/images/paper-SLC/SLC1.png)  
    - ![](/images/paper-SLC/SLC2.png)  
    - ![](/images/paper-SLC/SLC3.png)  
    - ![](/images/paper-SLC/SLC4.png)  
    - ![](/images/paper-SLC/SLC5.png)  

- At each step:
  - $w_j$: $j$-th smallest weight in the MST
  - $\mathrm{cost}_k$: sum of the costs of spanning trees within $k$ clusters

\[
\mathrm{cost}(G) := \sum_{k=1}^n \mathrm{cost}_k = \sum_{j=1}^n (n-j)w_j
\]

**Motivation:**
- $\mathrm{cost}_k$ captures cluster structure; SLC minimizes these costs.
- Naive MST computation requires $\tilde{O}(nd)$ time.
- **Question:** Can $\mathrm{cost}(G)$ and $\mathrm{cost}_k$ be estimated in sublinear time?

---

## Results

Let $W$ = max edge weight, $d$ = average degree. Query model: adjacency list.

| $\mathrm{cost}(G)$ | $\mathrm{cost}_k$ | Lower Bound         |
|--------------------|------------------|---------------------|
| $\tilde{O}(\sqrt{W}/\varepsilon^3 d)$ | $\tilde{O}(\sqrt{W}/\varepsilon^3 d)$ | $\Omega(\sqrt{W}/\varepsilon^2 d)$ |

**Succinct Representation:**  
For all $k$, can recover $\widehat{\mathrm{cost}_k}$ in short time, with average $(1+\varepsilon)$ error:
\[
\sum_{k=1}^n |\widehat{\mathrm{cost}_k} - \mathrm{cost}_k| \le \varepsilon \cdot \mathrm{cost}(G)
\]
Query time for retrieving each $k$: $O(\log\log W)$

---

## Proof Sketch

### Main Ideas

1. **Reduction:** SLC cost estimation reduces to estimating the number of connected components (CCs) in thresholded subgraphs.
2. **Estimating connected components:**  
   - Use sampling and BFS ([CRT05]).  
   - Visualization steps: ![](/images/paper-SLC/BFS0.png), ![](/images/paper-SLC/BFS1.png), ..., ![](/images/paper-SLC/BFS7.png)
   - Our algorithm improves on prior bounds.
3. **Binary search for acceleration:**  
   - Partition interval $[1,n]$ for CCs.
   - Map CC counts to nearest interval endpoints:  
     ![](/images/paper-SLC/map_intervals1.png) → ![](/images/paper-SLC/map_intervals2.png)
   - Choice of endpoints illustrated: ![](/images/paper-SLC/endpoints.png)

---

### Main Formula

\[
\mathrm{cost}(G) = \frac{n(n-1)}{2} + \frac{1}{2}\sum_{j=1}^{W-1}(c_j^2 - c_j)
\]
Where $c_j$ is the CC count in subgraph with edges $\leq j$.

### Profile Estimation

- Pre-processing: $\tilde{O}(\sqrt{W}/\varepsilon^3 d)$
- $\textsc{ProfileOracle}(k)$ returns $\widehat{\mathrm{cost}_k}$ in $O(\log\log W)$
- $\sum_{k=1}^n |\widehat{\mathrm{cost}_k} - \mathrm{cost}_k| = \varepsilon \mathrm{cost}(G)$

---

## Extension: Similarity Case

- **Input:** Weight represents similarity.
- **Method:** Merge most similar clusters.
- Adapted definitions: weight interpretation reversed; use MaxST instead of MST.

Step-by-step clustering example:
- ![](/images/paper-SLC/SLC_sim0.png)  
- ![](/images/paper-SLC/SLC_sim1.png)  
- ![](/images/paper-SLC/SLC_sim2.png)  
- ![](/images/paper-SLC/SLC_sim3.png)  

Profile calculations adapted accordingly.

| $\mathrm{cost}(G)$ | $\mathrm{cost}_k$ | Lower Bound           |
|--------------------|------------------|-----------------------|
| $\tilde{O}(W/\varepsilon^3 d)$ | $\tilde{O}(W/\varepsilon^3 d)$ | $\Omega(W/\varepsilon^2 d)$ |

Main formula:  
\[
\mathrm{cost}(G) = \sum_{j=1}^W \frac{(c_j + n - 1)(n - c_j)}{2}
\]

---

## Experiments

- $r$: sample size for CC estimation; $\widehat{\mathrm{cost}_k}$: estimated value for $\mathrm{cost}_k$.

**Illustrations:**
- Approximation ratio: ![](/images/paper-SLC/r_ratio_error_bar_combined.png)
- Estimated distance profiles: ![](/images/paper-SLC/estimated_combined_profile_distance.png)
- Estimated similarity profiles: ![](/images/paper-SLC/estimated_combined_profile_similarity.png)
- Time vs. sample size and profile:  
  ![](/images/paper-SLC/r_speed_up_combined_dist_log10.png)  
  ![](/images/paper-SLC/r_list_combined_speed_up_italy.png)  
  ![](/images/paper-SLC/r_speed_up_combined_sim.png)

---

## Conclusion

**Summary Table**

| Setting           | $\mathrm{cost}(G)$ | $\mathrm{cost}_k$ | Lower Bound         |
|-------------------|-------------------|-------------------|---------------------|
| Distance Case     | $\tilde{O}(\sqrt{W}/\varepsilon^3 d)$ | $\tilde{O}(\sqrt{W}/\varepsilon^3 d)$ | $\Omega(\sqrt{W}/\varepsilon^2 d)$ |
| Similarity Case   | $\tilde{O}(W/\varepsilon^3 d)$ | $\tilde{O}(W/\varepsilon^3 d)$           | $\Omega(W/\varepsilon^2 d)$       |

**Open Questions:**
- Dependency on $\varepsilon$
- Extension to other hierarchical clustering (average linkage, centroid linkage, etc.)

---

*Figures included above match those in the original slides. Mathematical notation is adapted for markdown where appropriate.*