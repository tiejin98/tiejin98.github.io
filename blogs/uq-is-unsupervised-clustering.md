---
layout: page
permalink: /blogs/uq-is-unsupervised-clustering/index.html
title: uq-is-unsupervised-clustering
---

# Position. LLM Uncertainty Quantification is Just Unsupervised Clustering

*Position paper "Uncertainty Quantification in LLMs is Just Unsupervised Clustering" (ICML 2026, Position track).*

Uncertainty quantification (UQ) is sold as the main safeguard for deploying LLMs in high-stakes settings. This paper argues that the field has a category error. Most mainstream UQ methods are, mechanically, unsupervised clustering algorithms. They measure how tightly a model's own generations agree, not whether those generations are correct. As a result they are blind to confident hallucinations, where a model is stable and certain about a wrong answer, and they hand practitioners a deceptive sense of safety.

**The position.** Semantic Entropy, graph and spectral methods, and verbalized P(true) all reduce to clustering. Semantic Entropy explicitly clusters generations into answer classes with an NLI model. Graph methods run implicit spectral clustering on a response-similarity Laplacian. P(true) is a soft membership test in the model's own belief space. Each one optimizes an internal geometric property, much like a Silhouette score, while staying blind to its agreement with reality, which would be the Adjusted Rand Index that nobody measures.

![Hidden states during P(true) form separated clusters](https://tiejin98.github.io/blog_image/uq-is-unsupervised-clustering/overview.png)

*A PCA view of Qwen2.5-32B hidden states during P(true) on QASC. High and low confidence samples fall into geometrically separated clusters, which is direct evidence that the method is doing clustering rather than checking truth.*

## Three pathologies that follow

1. A parameter sensitivity crisis. UQ scores shift with sampling count, NLI threshold, temperature, and prompt, and different methods even disagree about which samples are uncertain in the first place.
2. An internal evaluation trap. Evaluation rewards self-consistency, so stability gets mistaken for truth and confident hallucinations slip through.
3. A missing ground truth. True uncertainty is unobservable, so the field leans on noisy LLM judges and proxy correctness metrics, which creates a circular validation loop.

The clearest evidence sits in the disagreement numbers. Among the top uncertain samples flagged by different methods, the Jaccard overlap is tiny, for example 0.080 between Semantic Entropy and P(true) at the top 10 percent.

![Inter-method disagreement on QASC](https://tiejin98.github.io/blog_image/uq-is-unsupervised-clustering/results.png)

## The proposed roadmap

The authors call for a shift from internal heuristics to external validation along three pillars. First, evaluate worst-case behavior with tail-risk metrics and a stability curve rather than average AUROC. Second, change the mechanism by adopting conformal prediction and uncertainty alignment so confidence becomes native and set size explodes on hallucinations. Third, anchor verification in objective truth through unit tests in code and math and atomic fact checking against real sources, instead of asking another LLM to be the judge.

## Takeaway

If a UQ method never looks at the right answer, it is clustering, not measuring reliability. Treating LLM confidence as trustworthy requires grounding it in external truth, and this paper lays out how to start.
