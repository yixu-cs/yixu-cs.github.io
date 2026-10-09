---
title: "VehicleMemBench: An Executable Benchmark for Multi-User Long-Term Memory in In-Vehicle Agents"
collection: publications
category: conference
permalink: /publication/vehiclemembench
venue: "COLM 2026"
paperurl: 'https://arxiv.org/abs/2603.23840'
codeurl: 'https://github.com/MINE-USTC/VehicleMemBench'
dataseturl: 'https://huggingface.co/datasets/callalilya/VehicleMemBench'
posterurl: '/files/VehicleMemBench-poster.pdf'
excerpt: 'An executable benchmark that evaluates whether agents can use evolving, user-specific memories to perform correct tool actions. Task success is assessed through the final state of a vehicle simulator.'
authors: 'Yuhao Chen, Yi Xu, Xinyun Ding, Xiang Fang, Shuochen Liu, Luxi Lin, Qingyu Zhang, Ya Li, Quan Liu, Tong Xu'
---

**COLM 2026**

Yuhao Chen, Yi Xu, Xinyun Ding, Xiang Fang, Shuochen Liu, Luxi Lin, Qingyu Zhang, Ya Li, Quan Liu, Tong Xu.

[Code](https://github.com/MINE-USTC/VehicleMemBench) · [Dataset](https://huggingface.co/datasets/callalilya/VehicleMemBench) · [Poster](/files/VehicleMemBench-poster.pdf)

## Overview

Long-term agents need to remember whose preferences apply, when those preferences have changed, and which contextual conditions matter. VehicleMemBench evaluates these capabilities through executable tool interactions rather than recall questions alone.

The benchmark contains 500 queries across 50 multi-user scenarios. It covers preference conflicts, conditional constraints, coreference resolution, state shifts, and error correction. A vehicle simulator exposes 23 device modules and 111 executable APIs.

## Evaluation

We execute both agent-generated and reference actions from the same initial environment state. Exact State Match measures whether the final states agree, while field- and value-level scores distinguish selecting the right attributes from setting the right values.

Comparing autonomous memory with directly supplied gold memory reveals substantial performance gaps. Simple vehicle-focused summaries remain competitive with general-purpose memory systems, motivating better memory consolidation, retrieval, and handling of context and time.
