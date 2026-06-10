---
layout: page
permalink: /blogs/zer0-jack-blackbox-jailbreak/index.html
title: zer0-jack-blackbox-jailbreak
---

# Zer0-Jack: A Memory-efficient Gradient-based Jailbreaking Method for Black-box Multi-modal Large Language Models

Image-based jailbreaks of multi-modal LLMs are powerful, but the strongest ones need white-box access to model gradients, which commercial systems do not expose. This work introduces **Zer0-Jack**, a method that attacks the target directly in a black-box setting using only the output probabilities the model already returns. It is defensive security research that exposes a weakness so it can be fixed. You can read the full paper [here](https://aclanthology.org/2026.eacl-long.202).

---

## The Challenge: Strong Attacks Need Access That Real Systems Hide

White-box gradient methods achieve high success but require full parameter access, transfer attacks from a surrogate model lose much of their success on the real target, and naively estimating a gradient over the whole high-resolution image is both high variance and very memory heavy. A practical black-box attack has to avoid all three problems at once.

---

## Zer0-Jack: Patch-wise Zeroth-Order Optimization

The key idea is to replace the true image gradient with a zeroth-order estimate computed from two queries, and to optimize one small image patch at a time so the estimate stays accurate and the memory footprint stays small.

![The Zer0-Jack method overview](https://tiejin98.github.io/blog_image/zer0-jack-blackbox-jailbreak/overview.png)
*Zer0-Jack estimates a zeroth-order gradient from output probabilities and updates the adversarial image one patch at a time, so a billion-scale model can be attacked on a single consumer GPU.*

The method works in four steps.

1. Set the objective to force the model to begin its reply with an affirmative prefix, but optimize over the continuous image tensor instead of discrete text tokens.
2. Estimate the gradient with a two point zeroth-order formula that needs only the model's output probabilities.
3. Split the image into patches and, at each step, perturb and update a single patch through block coordinate descent, then move to the next patch and repeat.
4. Attack a commercial model directly by using the logit-bias option to make the target token's probability observable, which is enough to drive the optimization.

---

## Key Findings

On a harmful-behaviors multi-modal dataset, Zer0-Jack reaches a high attack success rate while staying close to the white-box upper bound, and it keeps working where gradient methods run out of memory.

![Attack success rate across models](https://tiejin98.github.io/blog_image/zer0-jack-blackbox-jailbreak/results.png)

It reaches 95 percent on one open model against transfer baselines near 13 to 16 percent, and 92 percent on a 70B model where white-box methods cannot run at all. Memory drops from 31G to 22G on the 13B model, and a direct attack on a commercial model lifts the success rate well above the text-only and unmodified-image settings.

---

## Why It Matters for Defenders

Black-box access plus output probabilities is enough to jailbreak a multi-modal LLM when the optimization is done patch by patch. Surfacing this efficient attack matters for defenders, since it shows that hiding gradients alone does not make a deployed model safe.
