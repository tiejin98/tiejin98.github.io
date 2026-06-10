---
layout: page
permalink: /blogs/zer0-jack-blackbox-jailbreak/index.html
title: zer0-jack-blackbox-jailbreak
---

# Zer0-Jack. A Memory-Efficient Black-Box Attack on Multi-modal LLMs

*Paper "Zer0-Jack: A Memory-Efficient Gradient-Based Jailbreaking Method for Black-Box Multi-modal Large Language Models" (ACL ARR). This is defensive security research that exposes a weakness so it can be fixed.*

Image-based jailbreaks of multi-modal LLMs are strong, but the strongest ones need white-box access to compute gradients through the model, which commercial systems do not expose. The usual fallback is a transfer attack from a surrogate model, and its success drops sharply on the real target. Zer0-Jack closes this gap by attacking the target directly in a black-box setting, using only the output probabilities the model already returns.

**The insight.** The image gradient can be replaced with a zeroth-order estimate computed from two queries, so no backpropagation and no model weights are needed. The key enabler is to optimize one small image patch at a time rather than the whole high-resolution image. Estimating a gradient over the full image is both high variance and memory heavy, while a 32 by 32 patch touches only about two percent of the dimensions, which sharply cuts both the estimation error and the memory footprint.

![The Zer0-Jack method overview](https://tiejin98.github.io/blog_image/zer0-jack-blackbox-jailbreak/overview.png)

*Zer0-Jack estimates a zeroth-order gradient from output probabilities and updates the adversarial image one patch at a time, so a billion-scale model can be attacked on a single consumer GPU.*

## How it works

1. Set the objective to force the model to begin its reply with an affirmative prefix, but optimize over the continuous image tensor instead of discrete text tokens.
2. Estimate the gradient with a two-point zeroth-order formula that needs only the model's output probabilities, which is an unbiased estimate of the true gradient.
3. Split the image into patches and, at each step, perturb and update a single patch through block coordinate descent, then move to the next patch and repeat for several passes.
4. Attack a commercial model directly by using the logit-bias option to make the target token's log-probability observable, which is enough to drive the optimization.

## What the experiments show

On the Harmful Behaviors multi-modal dataset, Zer0-Jack reaches a high attack success rate while staying close to the white-box upper bound, and it keeps working where gradient methods run out of memory.

![Attack success rate across models on Harmful Behaviors](https://tiejin98.github.io/blog_image/zer0-jack-blackbox-jailbreak/results.png)

It reaches 95 percent on MiniGPT-4 against transfer baselines near 13 to 16 percent, and 92 percent on a 70B model where the white-box and other baselines cannot run at all. Memory drops from 31G to 22G on the 13B model, the 70B model fits in 63G, and a direct attack on a commercial model lifts the success rate well above the text-only and unmodified-image settings.

## Takeaway

Black-box access plus output probabilities is enough to jailbreak a multi-modal LLM when the optimization is done patch by patch. Surfacing this efficient attack matters for defenders, since it shows that hiding gradients alone does not make a deployed model safe.
