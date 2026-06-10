---
layout: page
permalink: /blogs/ms-uq-multi-source-uncertainty/index.html
title: ms-uq-multi-source-uncertainty
---

# Uncertainty Quantification of Large Language Models through Multi-Dimensional Responses

Every uncertainty quantification method for LLMs looks at one signal, yet a fluent answer can still be wrong, so any single signal gives an incomplete picture. This work introduces **MS-UQ**, a black-box framework that combines several heterogeneous uncertainty signals into one score without letting redundant agreement dominate. You can read the full paper [here](https://arxiv.org/abs/2502.16820).

---

## The Challenge: Each Signal Tells Only Part of the Story

Different uncertainty sources, such as semantic entailment, response graph statistics, and claim level factual consistency, overlap a lot. Simply averaging or concatenating them mostly recounts the same shared structure and masks the differences that actually matter, so a good framework has to disentangle what is shared from what is unique and stay extensible to new signals.

---

## MS-UQ: Fusing Uncertainty Sources with a Tensor

The key idea is that consistent responses compress into a low-rank tensor, while disagreement across sources resists compression, so reconstruction error becomes the uncertainty.

![The MS-UQ pipeline](https://tiejin98.github.io/blog_image/ms-uq-multi-source-uncertainty/overview.png)
*Each uncertainty source becomes a response by response similarity matrix, the matrices stack into a third order tensor, and tensor decomposition separates shared from source specific structure so reconstruction error reads out as uncertainty.*

The method works in four steps.

1. For the sampled responses to a question, build one similarity matrix per source.
2. Stack the matrices as slices of a tensor over response, response, and source, which keeps each source orthogonal rather than blending them.
3. Decompose the tensor two complementary ways, Tucker for expressiveness and CP for noise robustness, while leaving the source mode uncompressed so source specific signal survives.
4. Read uncertainty from the relative reconstruction error, summed across ranks, then combine the two decompositions with a tuning free rule so new sources can be added without redesign.

---

## Key Findings

MS-UQ with the sum ensemble is the best method in every model and dataset cell, across five LLMs on three question answering datasets.

![Main results across five models and three datasets](https://tiejin98.github.io/blog_image/ms-uq-multi-source-uncertainty/results.png)

The gains are largest on the reasoning heavy dataset, and a supporting analysis confirms the mechanism, since higher accuracy samples consistently produce lower reconstruction error.

---

## Takeaway

Uncertainty signals are complementary but redundant, so they should be disentangled rather than averaged. A tensor over sources, read through its reconstruction error, fuses them into one scalable score that beats every single source baseline.
