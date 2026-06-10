---
layout: page
permalink: /blogs/cfa-conformal-feedback-alignment/index.html
title: cfa-conformal-feedback-alignment
---

# Conformal Feedback Alignment: Quantifying Answer-Level Reliability for Robust LLM Alignment

Reinforcement learning from AI feedback replaces costly human labels with model generated preferences, but that feedback is noisy. This work introduces **CFA**, which measures how reliable each individual answer is with conformal prediction and folds that reliability into the alignment loss. You can read the full paper [here](https://aclanthology.org/2026.findings-eacl.184/).

---

## The Challenge: A Preference Is Only as Good as Its Two Answers

Prior uncertainty aware methods try to handle noisy feedback by weighting the preference, that is by asking how reliable the comparison between two answers is. This overlooks a simpler blind spot, because if both compared answers are low quality, the preference carries little information no matter how the comparison is modeled. Reliability should be measured at the answer level, not only at the preference level.

---

## CFA: Weighting Feedback by Conformal Reliability

The key idea is to estimate, with distribution free guarantees, how reliable each answer is, then let weak pairs contribute a weaker learning signal.

![The CFA pipeline](https://tiejin98.github.io/blog_image/cfa-conformal-feedback-alignment/overview.png)
*Conformal prediction scores each answer in both black-box and white-box settings, the pair's set memberships collapse to a single weight, and that weight scales the alignment loss.*

The method works in four steps.

1. Sample answers and obtain pairwise preferences from an AI evaluator.
2. Score each answer with a nonconformity measure, using negative log likelihood in the white-box case and a frequency and entropy based score in the black-box case.
3. Calibrate conformal sets at two coverage levels on a held out set, then aggregate the pair's set memberships into one weight that is lower when the answers fall in weaker or mismatched sets.
4. Multiply the alignment loss by that weight, so unreliable pairs move the policy less.

---

## Key Findings

CFA improves on the base alignment in every model, dataset, and algorithm combination, across three models on three datasets and both reward-model and direct preference optimization.

![CFA versus base alignment](https://tiejin98.github.io/blog_image/cfa-conformal-feedback-alignment/results.png)

The strongest single point gains as much over the base as the base itself gained over plain supervised fine-tuning, and CFA also beats preference-level uncertainty methods.

---

## Takeaway

Noisy AI feedback is partly a data quality problem at the source. Measuring how reliable each answer is, then weighting the learning signal by it, makes alignment more robust and more data efficient than weighting the preference alone.
