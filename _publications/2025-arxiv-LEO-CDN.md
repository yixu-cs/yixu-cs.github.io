---
title: "Replica Server Placement in a Satellite Network"
collection: publications
category: manuscripts
permalink: /publication/2025-arxiv-LEO-CDN
excerpt: 'Satellites can be applied as content replica servers to improve client experience in the remote locations. We optimized the placement of servers in satellite networks to reduce transmission and storage costs while considering satellite movement.'
date: 2025-08-01
venue: 'Arxiv'
slidesurl: '/files/LEO%20applications%20-%20Yi.pptx'
paperurl:  '/files/preprint-SatCDN.pdf'
authors: Zhiyuan He, <b>Yi Xu</b>, Cheng Luo, Lili Qiu, Yuqing Yang
---

Authors: Zhiyuan He, <b>Yi Xu</b>, Cheng Luo, Lili Qiu, Yuqing Yang

Motivation
------

Low Earth Orbit (LEO) satellites have low latency and wide coverage capabilities, and thus they have various applications on global connectivity. Our paper apply LEO satellites with Content Delivery Networks (CDN) for improving internet infrastructure, especially in remote regions.

Conventional CDNs rely on terrestrial networks and distributed data centers to cache and deliver content efficiently to users worldwide. However, these networks face challenges such as latency, limited coverage in remote areas, and high infrastructural costs. LEO satellite networks address these limitations by providing:

- **Global coverage:** LEO constellations ensure connectivity even in remote or rural locations.
- **Low latency:** With altitudes around 500–2000 km, LEO satellites offer much lower latency compared to GEO satellites.
- **Frequency re-use:** LEO satellites only see a small part of the Earth's surface, and thus they are good for frequency re-use.

Challenges
------

Deploying CDN replicas in LEO satellite networks involves unique challenges:

- **Dynamic topology:** Satellites move quickly, and their coverage areas constantly change. When a LEO satellite moves out of range of terrestrial gateway and terminal, this satellite need to **handover** its content to next satellite.

<p align="center">
  <img src="/images/paper-CDN/handover.png" width="450"/>
</p>

- **Replica placement:** Deciding where and when to place content replicas to optimize delivery.
- **Client coverage:** Ensuring every client can efficiently reach a content replica as satellites and users move.

Algorithms
------

This problem can be formalized as **Facility Location Problem**:
Among all the LEO satellites, we choose and *open* several of them as facilities (CDN replicas). And the opened facilities have connection and handover cost when they are serving the users. Then, our goal is to **minimize the sum of open cost and connection cost**.

### Greedy facility location algorithms

[Reference](https://arxiv.org/pdf/cs/0207028)

The <u>original greedy algorithm</u> runs as follows:

1. Initialize opened facility set and unassigned client set to be empty
2. While there exists clients who are not unassigned:
  - Pick a facility $$i$$ and an unassigned client set $$Y$$ such that $$\frac{\text{open cost for }i+\text{net connection cost}}{\text{size of }Y}$$ is minimized
  - **Net connection cost** is defined as the total connection cost incurred when assigning the client set $$Y$$ to facility $$i$$, *minus the potential cost savings* from reassigning any clients who have already been assigned to another facility to facility $$i$$ instead.

While this algorithm is easy to understand, it cost exponential in time to pick the client set $$Y$$. On the other hand, we can utilize the primal-dual property, and design another <u>algorithm for the dual problem</u>:

1. Increase clients' budget ("money") over time.
2. Connect to an existing replica if possible; otherwise, open a new replica when the budget allows.
3. Continue until all clients are covered.

This approach balances between opening minimal replicas and ensuring all clients in the satellite footprint are served, even as coverage areas evolve due to satellite motion.

### Local search facility location algorithms

[Reference](https://www.cs.dartmouth.edu/~deepc/LecNotes/Appx/2b.%20Local%20Search%20Algorithms%20for%20Facility%20Location.pdf)

### Dynamic programming

