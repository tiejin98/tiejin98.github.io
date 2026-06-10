---
layout: page
permalink: /blogs/matu-multi-agent-uncertainty/index.html
title: matu-multi-agent-uncertainty
---

# Every Response Counts. Measuring Uncertainty in LLM Multi-Agent Systems

*Paper "Every Response Counts: Quantifying Uncertainty of LLM-based Multi-Agent Systems through Tensor Decomposition" (ACL). Code at https://github.com/tiejin98/MATU*

LLM multi-agent systems (MAS) beat single agents on hard tasks, yet we still cannot tell when their answers are trustworthy. Existing uncertainty quantification (UQ) was designed for one model answering one question once. A MAS instead produces many reasoning steps, many communication paths between agents, and runs under different topologies such as star, chain, or dynamic routing. Looking only at the final text throws away almost everything that makes a MAS uncertain.

**The core idea.** If a multi-agent system is reliable, its repeated runs should reach the same place through similar reasoning, so their embedded trajectories share a common low-rank structure. The harder those trajectories are to compress into that shared structure, the more the agents actually disagree, and the higher the true uncertainty. MATU (Multi-Agent Tensor Uncertainty) turns this intuition into a measurement.

![The MATU pipeline](https://tiejin98.github.io/blog_image/matu-multi-agent-uncertainty/overview.png)

*The MATU pipeline. Every run of the system produces step-by-step reasoning, each step becomes an embedding, the runs stack into a ragged tensor over steps, agents, and runs, and the reconstruction error of a low-rank decomposition becomes the uncertainty score.*

## How it works

1. Run the same multi-agent system on a task ten times at temperature 0.9 and record every agent's full step-by-step reasoning, including tool outputs.
2. Embed each step with a pretrained text embedding model, so each run of each agent becomes a matrix of step embeddings.
3. Stack all matrices into a three-dimensional ragged tensor whose modes are steps, agents, and runs. The ragged form absorbs trajectories of different lengths, which is exactly where prior methods break.
4. Apply PARAFAC2 (CP-2) decomposition to recover the expected semantic consensus across runs.
5. Sum the reconstruction error across ranks. A larger residual means trajectories deviate more from the shared consensus, so the uncertainty is higher.

The appendix grounds this further by showing the CP-2 reconstruction loss is mathematically the aggregated squared deviation from the consensus, so MATU is a principled estimate of ensemble variance rather than a heuristic.

## What the experiments show

MATU was tested on three MAS frameworks (Camel, AutoGen, AnyMac) with GPT-4o, Qwen2.5-7B, and Llama3.1-8B over MATH, MoreHopQA, MMLU, and HumanEval for tool use. It gives the best AUROC and AUARC almost everywhere. On MATH with Llama3.1-8B, AUROC reaches 0.7354 while the strongest baseline sits near 0.56.

![Main results on the Camel framework](https://tiejin98.github.io/blog_image/matu-multi-agent-uncertainty/results.png)

A case study makes the failure of baselines concrete. On a geometry question that all ten runs answer correctly, the length-sensitive baseline SAUP reports high uncertainty (0.88) simply because some runs are verbose, while MATU correctly reports low uncertainty (0.05) because it aligns the reasoning across runs regardless of surface length.

## Takeaway

Reliability in a multi-agent system lives in the whole reasoning process, not the final sentence. Representing that process as a tensor and reading uncertainty from how well it compresses gives one robust score that holds across models, datasets, and communication topologies.
