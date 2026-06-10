---
layout: page
permalink: /blogs/matu-multi-agent-uncertainty/index.html
title: matu-multi-agent-uncertainty
---

# Every Response Counts: Quantifying Uncertainty of LLM-based Multi-Agent Systems through Tensor Decomposition

LLM multi-agent systems beat single agents on hard tasks, yet we still cannot tell when their answers are trustworthy. This work introduces **MATU** (Multi-Agent Tensor Uncertainty), a framework that measures uncertainty directly from the full reasoning trajectories of a multi-agent system rather than from its final text. You can read the full paper [here](https://arxiv.org/abs/2604.08708).

---

## The Challenge: Uncertainty Lives in the Whole Reasoning Process

Existing uncertainty quantification was designed for one model answering one question once. A multi-agent system instead produces many reasoning steps, many communication paths between agents, and runs under different topologies such as star, chain, or dynamic routing. Reading only the final answer throws away almost everything that makes such a system uncertain, so a method built for single answers cannot capture cascading errors or disagreement between agents.

---

## MATU: Reading Uncertainty from a Tensor of Trajectories

The core intuition is that a reliable system reaches the same place through similar reasoning, so its repeated runs share a common low-rank structure. The harder those runs are to compress into that shared structure, the more the agents disagree, and the higher the uncertainty.

![The MATU pipeline](https://tiejin98.github.io/blog_image/matu-multi-agent-uncertainty/overview.png)
*The MATU pipeline turns each run into embedding matrices, stacks them into a ragged tensor over steps, agents, and runs, and reads uncertainty from the reconstruction error of a low-rank decomposition.*

The method works in five steps.

1. Run the system ten times on a task and record every agent's step-by-step reasoning, including tool outputs.
2. Embed each step with a pretrained text embedding model, so each run of each agent becomes a matrix of embeddings.
3. Stack the matrices into a three-dimensional ragged tensor whose modes are steps, agents, and runs, which absorbs trajectories of different lengths.
4. Apply PARAFAC2 (CP-2) decomposition to recover the shared consensus across runs.
5. Sum the reconstruction error across ranks and use it as the uncertainty score, where a larger residual means more disagreement and lower reliability.

---

## Key Findings

MATU was tested on three multi-agent frameworks and three models across math, multi-hop QA, and general reasoning. It gives the best AUROC and AUARC almost everywhere, and the gains are largest on harder reasoning tasks.

![Main results](https://tiejin98.github.io/blog_image/matu-multi-agent-uncertainty/results.png)

On math with Llama3.1-8B, AUROC reaches 0.7354 while the strongest baseline stays near 0.56. A case study makes the difference concrete. A length-sensitive baseline flags a fully correct system as highly uncertain simply because some runs are verbose, while MATU correctly reports low uncertainty by aligning the reasoning across runs.

---

## Why It Matters

Reliability in a multi-agent system lives in the whole reasoning process, not the final sentence. Representing that process as a tensor and reading uncertainty from how well it compresses gives one robust score that holds across models, datasets, and communication topologies.
