---
layout: page
permalink: /blogs/llm-uncertainty-quantification-survey/index.html
title: llm-uncertainty-quantification-survey
---

# Uncertainty Quantification and Confidence Calibration in Large Language Models: A Survey

Large language models produce fluent answers that are often wrong, so knowing how confident a model should be is essential before trusting it. This survey reorganizes uncertainty quantification for LLMs around where uncertainty actually enters a response, and gives a taxonomy that practitioners can use to pick a method. You can read the full paper [here](https://arxiv.org/pdf/2503.15850).

---

## The Gap: Classical Uncertainty Theory Misses LLMs

Prior surveys focus narrowly on hallucination detection or retrofit classical taxonomies that classify methods by input level or technique. The classical split into aleatoric and epistemic uncertainty cannot capture LLM specific sources such as prompt ambiguity, reasoning path divergence, and decoding stochasticity, and traditional estimators like Monte Carlo dropout are too expensive at scale.

---

## A Taxonomy Built Around the Generation Process

The survey proposes two axes, one for computational efficiency that separates single pass from sampling based from extra-model and fine-tuning methods, and one for where the uncertainty originates.

![Taxonomy of methods across dimensions, efficiency, and model access](https://tiejin98.github.io/blog_image/llm-uncertainty-quantification-survey/taxonomy.png)
*Representative methods organized by uncertainty dimension, efficiency, model access, and whether they also yield confidence, which doubles as a quick selection guide.*

The second axis has four dimensions.

1. Input uncertainty, from ambiguous prompts, probed by perturbing or paraphrasing the input.
2. Reasoning uncertainty, in multi-step chains and retrieval, tracked through explanation graphs or token probabilities along the path.
3. Parameter uncertainty, from gaps in the weights, estimated by Bayesian and fine-tuning approaches.
4. Prediction uncertainty, the most studied, spanning perplexity and entropy, conformal prediction, consistency measures, semantic entropy, and spectral graph methods.

---

## Open Challenges

Four problems stand out. Multi-sample methods are costly for small gains, so cheaper proxies are needed. Scores are hard to interpret, so the source of uncertainty should be decoupled. Uncertainty in agents and long reasoning chains accumulates and is rarely tracked. Evaluation itself is biased and does not capture meaningful uncertainty such as confidently wrong answers.

---

## Takeaway

Uncertainty in an LLM is not one number from one place. Organizing methods by where uncertainty arises and how much compute they need turns a crowded literature into a practical map and points clearly at the gaps worth working on next.
