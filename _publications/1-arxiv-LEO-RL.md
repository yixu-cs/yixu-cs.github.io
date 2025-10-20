---
title: "Joint Optimization of Handoff and Video Rate in LEO Satellite Networks"
collection: publications
category: manuscripts
permalink: /publication/1-arxiv-LEO-RL
excerpt: 'We propose and evaluate new algorithms for video-aware mobility management in satellite networks, jointly optimizing satellite handoff and video bitrate to improve streaming experience for both single and multiple users.'
date: 2023-04-06
venue:
slidesurl: 
paperurl: 'https://arxiv.org/abs/2504.04586'
authors: Kyoungjun Park, Zhiyuan He, Cheng Luo, Yi Xu<sup>*</sup>, Lili Qiu, Changhan Ge, Muhammad Muaz, Yuqing Yang
---

Authors: Kyoungjun Park, Zhiyuan He, Cheng Luo, Yi Xu<sup>*</sup>, Lili Qiu, Changhan Ge, Muhammad Muaz, Yuqing Yang


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
2. **Evaluate current satellite:** Use MPC algorithm to compute the expected QoE and recommended bitrate for satellite $$c$$, based on historical bitrate, buffer size, and the predicted throughput. Record $$c$$ and its associated metrics as the current best choice
3. **Explore visible satellites:**  
  For each visible satellite $$i$$ in the current time step:
  - Predict the future throughput of $$i$$
  - Use MPC algorithm to compute the expected QoE and recommended bitrate for satellite $$i$$, also considering the handoff point from the current satellite
  - If $$i$$ yields a higher QoE than the current best, update the best satellite to $$i$$ (along with its corresponding bitrate and QoE)
4. **Output optimal configuration:** Return the best QoE, corresponding bitrate, selected sattelite and the chosen handoff point as the decision for the current time step

### RL-based algorithm

We apply Proximal Policy Optimization (PPO), a state-of-the-art actor/critic method, which trains two networks: a critic network to estiamte the value function and an actor network to optimize the policy based on the value function.

The figure below shows our networks, where $$S$$ denotes the state space after downloading a chunk $$t$$, $$v$$ is the value function, $$a$$ is the action space, and $$\pi$$ represents the sate transition probability.

<p align="center">
  <img src="/images/paper-RL/PPO network.png" width="450"/>
</p>

* **State:** The state fed into the actor and critic networks includes:
  - Current buffer level
  - Number of chunks remaining in the video
  - Bitrate of the last downloaded chunk
  - A vector of $$m$$ available sizes for the next video chunk
  - Download time for the past $$k$$ chunks (time interval for throughput measurements)
  - Throughput measurements from two satellites over the past $$k$$ chunks
  - Visible time elapsed
  - Current satellite and runner-up satellite (critical for joint decision of bitrate and handoff)

* **Action:** Choose both the satellite and the video bitrate for the next chunk at the upcoming time step
* **Reward:** The reward is defined as the video QoE metric, which we aim to optimize
* **Policy:** The policy specifies the probability of taking each possible action in a given state
* **Policy update/training process:**
  - Collect user state data
  - Calculate individual QoE
  - Adjust the hyperparameters (e.g., learning rates and model weights)
  - Re-calculate QoE to ensure the optimization does not degrade other users' QoE
  - Make decisions based on the updated QoE scores

In particular, our RL method employs centralized training with distributed inference: the model is trained with access to global network and user states but infers decisions independently for each user in deployment.

Experiments
------

We evaluate our approach against popular satellite selection strategies and Adaptive Bitrate (ABR) algorithms. The baseline methods include:
* Maximum visible time (MVT): Switch to the satellite with the longest visible time when the currently connected satellite is about to leave the client's horizon
* Maximum received signal strength (MRSS): Switch to the satellite with the highest signal strength
* Maximum bandwidth (MB): Switch to the satellite with the maximum available bandwidth

The figure below shows QoE results on simulated, NOAA and Starlink datasets. The number of users refers to how many users are streaming video out of 20 users.

| QoE for different datasets | 
|:--:| 
| <img src='/images/paper-RL/experiment-QoE-datesets.png' width='600' /> |

The next figure represents a breakdown of QoE on the simulated dataset, showing quality reward, rebuffer penalty and smoothness penalty.

| QoE breakdown results | 
|:--:| 
| <img src='/images/paper-RL/experiment-QoE-breakdown.png' width='600' /> |
