---
layout: page
permalink: /blogs/llm-uncertainty-quantification-survey/index.html
title: llm-uncertainty-quantification-survey
---

# A Survey of Uncertainty Quantification and Confidence Calibration in LLMs

*Paper "Uncertainty Quantification and Confidence Calibration in Large Language Models: A Survey" (KDD 2025).*

Large language models produce fluent answers that are often wrong, so knowing how confident a model should be is essential before trusting it. Classical uncertainty theory splits uncertainty into aleatoric and epistemic, but that view misses where uncertainty actually enters an LLM response. This survey reorganizes the field around the LLM generation process itself and gives a taxonomy that practitioners can use to pick a method.

**The contribution.** The survey proposes two axes. One axis is computational efficiency, which separates single-pass methods from sampling-based methods, from methods that need an extra model, from methods that require fine-tuning. The other axis is where the uncertainty originates, namely input, reasoning, parameter, and prediction uncertainty. Any method lands at a point on both axes, which makes the trade-off between cost and coverage explicit.

![Taxonomy of UQ methods across dimensions, efficiency, and model access](https://tiejin98.github.io/blog_image/llm-uncertainty-quantification-survey/taxonomy.png)

*Representative UQ methods organized by uncertainty dimension, efficiency, model access, and whether they also yield confidence. The table doubles as a quick selection guide.*

## The four uncertainty dimensions

1. Input uncertainty comes from ambiguous prompts. Methods perturb or paraphrase the input and measure how the response changes. This area is still understudied.
2. Reasoning uncertainty lives in multi-step chains and retrieval. Methods track explanation graphs, chain-of-thought signals, or token probabilities along the reasoning path.
3. Parameter uncertainty reflects gaps in the weights. Bayesian LoRA and fine-tuning approaches estimate it, while classical Monte Carlo dropout and deep ensembles are too expensive at scale.
4. Prediction uncertainty captures variability across generations and is the most studied. It spans perplexity and entropy, conformal prediction, consistency measures, semantic entropy, and spectral graph methods.

## Evaluation, applications, and open problems

The survey frames UQ evaluation as separating correct from incorrect answers, reports AUROC, AUPRC, and AUARC as the main metrics, and discusses calibration error and the pitfalls of LLM-as-judge scoring for open-ended text. It collects benchmarks across reading comprehension, reasoning, factuality, and ambiguity, and reviews applications in robotics, transportation, and healthcare.

Four open challenges stand out. Multi-sample UQ is costly for small gains, so cheaper proxies are needed. Scores are hard to interpret, so the source of uncertainty should be decoupled. Uncertainty in agents and long reasoning chains accumulates and is rarely tracked. Evaluation itself is biased and does not capture meaningful uncertainty such as confidently wrong answers.

## Takeaway

Uncertainty in an LLM is not one number from one place. Organizing methods by where uncertainty arises and how much compute they need turns a crowded literature into a practical map, and it points clearly at the gaps worth working on next.
