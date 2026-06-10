---
layout: page
permalink: /blogs/ms-uq-multi-source-uncertainty/index.html
title: ms-uq-multi-source-uncertainty
---

# MS-UQ. Fusing Many Uncertainty Signals with a Tensor

*Paper "Uncertainty Quantification of Large Language Models through Multiple Uncertainty Sources" (ACL ARR). Code at https://github.com/tiejin98/MSUQ*

Every uncertainty quantification method for LLMs looks at one signal. Some compare answers with natural language inference, some build response-similarity graphs, some check claim-level factual consistency. A fluent answer can still be wrong, so any single signal gives an incomplete picture. MS-UQ combines several heterogeneous signals into one black-box uncertainty score, and it does so without averaging them, which would let redundant agreement dominate.

**The insight.** Different uncertainty sources overlap a lot. The paper measures a spectral norm ratio of 0.89 between semantic and claim-level signals, which means naive concatenation mostly re-counts the same shared structure and masks the differences that matter. Instead, stack the sources as slices of a third-order tensor. Consistent responses compress into a low-rank tensor, while disagreement across sources resists compression, so the reconstruction error becomes the uncertainty.

![The MS-UQ pipeline](https://tiejin98.github.io/blog_image/ms-uq-multi-source-uncertainty/overview.png)

*Each uncertainty source becomes a response-by-response similarity matrix, the matrices stack into a three-dimensional tensor, and tensor decomposition separates shared from source-specific structure so the reconstruction error reads out as uncertainty.*

## How it works

1. For the sampled responses to a question, build one similarity matrix per source, including semantic entailment, graph statistics, and claim-level consistency.
2. Stack the matrices as slices of a tensor over response, response, and source, which keeps each source orthogonal rather than blending them.
3. Decompose the tensor two complementary ways, Tucker for expressiveness and CP for noise robustness, while leaving the source mode uncompressed so source-specific signal survives.
4. Read uncertainty from the relative reconstruction error. A theorem shows more disagreement among sources forces more rank-one structure and therefore a larger error.
5. Sum errors across ranks to avoid sensitivity to a single rank, then combine the CP and Tucker scores with a tuning-free sum or min, so new sources can be added without redesign.

## What the experiments show

MS-UQ with the sum ensemble is the best method in every model and dataset cell, across five LLMs on CoQA, HotpotQA, and NQ_Open.

![Main results across five models and three datasets](https://tiejin98.github.io/blog_image/ms-uq-multi-source-uncertainty/results.png)

The gains are largest on reasoning-heavy HotpotQA, for example AUROC rising to 0.6588 on Llama3.1-8B against 0.6362 for the best baseline. A supporting analysis confirms the mechanism, since higher-accuracy samples consistently produce lower reconstruction error.

## Takeaway

Uncertainty signals are complementary but redundant, so they should be disentangled rather than averaged. A tensor over sources, read through its reconstruction error, fuses them into one scalable score that beats every single-source baseline.
