---
title: "Joint Optimization of Handoff and Video Rate in LEO Satellite Networks"
collection: publications
category: manuscripts
permalink: /publication/2025-arxiv-LEO-RL
excerpt: 'We propose and evaluate new algorithms for video-aware mobility management in satellite networks, jointly optimizing satellite handoff and video bitrate to improve streaming experience for both single and multiple users.'
date: 2025-04-06
venue: 'Arxiv'
slidesurl: 
paperurl: 'https://arxiv.org/abs/2504.04586'
authors: Kyoungjun Park, Zhiyuan He, Cheng Luo, <b>Yi Xu</b>, Lili Qiu, Changhan Ge, Muhammad Muaz, Yuqing Yang
---

Authors: Kyoungjun Park, Zhiyuan He, Cheng Luo, <b>Yi Xu</b>, Lili Qiu, Changhan Ge, Muhammad Muaz, Yuqing Yang


Motivation and challenges
------

Since Low Earch Orbit (LEO) satellites provide global network coverage at an affordable price and they have lower latency and higher throughput compared to geostationary satellites, they can be utilized in video streaming.

Video streaming needs to adaptively select bitrate according to the bandwidth of internet. Besides, as satellites constantly move and users may traverse different coverage zones, throughput prediction and resource allocation are difficult.

Therefore, we need to develop novel algorithms that jointly manage satellite selection and adaptive bitrate decisions. Our optimization goal is Quality on Experience (QoE) metric, which measures a user's perceived satisfaction with the video.

Algorithms
------

The proposed approaches leverage Model Predictive Control (MPC) and Reinforcement Learning (RL), designed for both single-user and multi-user cases. 

### Joint MPC algorithm

The MPC algorithm is effective for video bitrate adaption on the Internet. For each user during each time step, we perform the following procedure to select satellite and bitrate for video streaming:

1. **Predict throughput of current satellite:** predict the future throughput of the currently connected satellite $$c$$, using a prediction method such as the harmonic mean over historical throughput
2. **Evaluate current satellite:** Use MPC algorithm to compute QoE and recommended bitrate for satellite $$c$$, based on historical bitrate, buffer size, and the predicted throughput. Record $$c$$ and its associated metrics as the current best choice
3. **Explore visible satellites:**  
  For each visible satellite $$i$$ in the current time step:
  - Predict the future throughput of $$i$$
  - Use MPC algorithm to compute QoE and bitrate of satellite $$i$$, considering handoff point
  - If $$i$$'s QoE is better than the best QoE up till now, then replace the best satellite as $$i$$
4. Return the best QoE, bitrate, correspondent sattelite and handoff point as algorithms' decision

### RL-based algorithm

In particular, our RL method employs centralized training with distributed inference: the model is trained with access to global network and user states but infers decisions independently for each user in deployment. This design balances learning efficiency and practical scalability, allowing for explicit consideration of satellite and ground station interactions while supporting distributed, real-world inference. We extend our RL algorithm with support for multiple bitrate levels and dynamic resource allocation mechanisms to simulate real-world application diversity.

Experiments
------
 
We conduct comprehensive evaluations of the proposed algorithms using trace-driven simulations and a real-world testbed. Video streaming scenarios are tested with various bitrate levels and realistic user mobility patterns, split into chunks transmitted via LEO networks. Both single-user and multi-user experiments are performed, including dynamic resource allocation among heterogeneous applications. Our results highlight that joint optimization of satellite selection and bitrate adaptation significantly improves user QoE—up to 68% compared to baseline separate optimization strategies. We further analyze the impact of RL design choices, resource sharing strategies, and geographic locations, demonstrating the effectiveness and practicality of our methods for improving video streaming and general user experience in LEO satellite environments.
