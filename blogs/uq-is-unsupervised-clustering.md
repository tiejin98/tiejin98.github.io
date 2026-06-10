---
layout: page
permalink: /blogs/uq-is-unsupervised-clustering/index.html
title: uq-is-unsupervised-clustering
---

# Position: Uncertainty Quantification in LLMs is Just Unsupervised Clustering

Uncertainty quantification is sold as the main safeguard for deploying LLMs in high-stakes settings. This position paper argues that the field has a category error, because most mainstream methods are mechanically unsupervised clustering algorithms that measure how tightly a model's own generations agree rather than whether they are correct. You can read the full paper [here](https://arxiv.org/abs/2605.19220).

---

## The Core Problem: Measuring Agreement Instead of Truth

Semantic Entropy, graph and spectral methods, and verbalized confidence all reduce to clustering. Semantic Entropy explicitly clusters generations into answer classes with an entailment model, graph methods run implicit spectral clustering on a response similarity graph, and verbalized confidence is a soft membership test in the model's own belief space. Each one optimizes an internal geometric property while staying blind to its agreement with reality, so a model can be stable and certain about a wrong answer and still look perfectly confident.

![Hidden states form separated clusters during a confidence test](https://tiejin98.github.io/blog_image/uq-is-unsupervised-clustering/overview.png)
*A PCA view of model hidden states during a confidence test, where high and low confidence samples fall into geometrically separated clusters. This is direct evidence that the method is doing clustering rather than checking truth.*

---

## The Argument: Three Pathologies of Internal Uncertainty

1. A parameter sensitivity crisis, where scores shift with sampling count, entailment threshold, temperature, and prompt, and different methods even disagree about which samples are uncertain.
2. An internal evaluation trap, where evaluation rewards self consistency, so stability is mistaken for truth and confident hallucinations slip through.
3. A missing ground truth, where the field leans on noisy model judges and proxy correctness metrics, which creates a circular validation loop.

The disagreement is concrete. Among the top uncertain samples flagged by different methods, the overlap is tiny.

![Methods disagree on which samples are uncertain](https://tiejin98.github.io/blog_image/uq-is-unsupervised-clustering/results.png)

---

## A Roadmap Toward External Validation

The authors call for a shift from internal heuristics to external validation along three pillars. Evaluate worst case behavior with tail risk metrics and a stability curve rather than average scores. Change the mechanism by adopting conformal prediction and uncertainty alignment so confidence becomes native. Anchor verification in objective truth through unit tests in code and math and atomic fact checking against real sources, instead of asking another model to be the judge.

---

## Takeaway

If a method never looks at the right answer, it is clustering, not measuring reliability. Treating LLM confidence as trustworthy requires grounding it in external truth.
