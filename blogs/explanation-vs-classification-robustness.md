---
layout: page
permalink: /blogs/explanation-vs-classification-robustness/index.html
title: explanation-vs-classification-robustness
---

# Are Classification Robustness and Explanation Robustness Really Strongly Correlated? An Analysis Through Input Loss Landscape

A common belief says that a model robust to adversarial attacks also has robust explanations, since both should come from a flat input loss landscape. This work tests that belief and finds the two are decoupled. You can read the full paper [here](https://arxiv.org/abs/2403.06013).

---

## The Challenge: A Widely Assumed Correlation That May Not Hold

If the belief were true, robust classifiers should also have flat explanation loss landscapes and robust saliency maps. The risk is real, because a saliency map can be adversarially manipulated without changing the predicted label, which opens a security gap if the assumed correlation is false.

![A saliency map manipulated while the prediction stays fixed](https://tiejin98.github.io/blog_image/explanation-vs-classification-robustness/overview.png)
*Adding crafted noise leaves the predicted label unchanged yet steers the saliency map toward a target map, which is the manipulation that motivates the study.*

---

## The Analysis and the SEP Method

The central finding is that classification robustness arises from a flat input loss landscape, but explanation robustness arises mainly from a high starting explanation loss, so flatness is the wrong lever for explanations.

1. Control classification robustness with a standard trade-off training method, sweeping its weight to span a range of adversarial accuracy.
2. Make evaluation tractable with cluster based sampling of prototypes, then measure explanation robustness by the explanation loss after an attack converges.
3. Visualize the input loss landscape of that explanation loss and find no flatness difference across robustness levels.
4. Introduce SEP, which adds a regularizer on the explanation under random noise, where a positive weight flattens the landscape and a negative weight sharpens it.

---

## Key Findings

Across five datasets and two architectures, the sharpening variant always gives the strongest explanation robustness and the flattening variant the weakest, while adversarial accuracy barely moves.

![Explanation and classification robustness move independently](https://tiejin98.github.io/blog_image/explanation-vs-classification-robustness/results.png)

On one setting the explanation robustness varies by roughly four and a half times between the two variants, yet adversarial accuracy stays nearly constant, and a Hessian analysis confirms the landscape effect.

---

## Takeaway

Robust classification does not buy robust explanations. Since the two can be moved independently, defending a model's predictions and defending its explanations are separate goals, and saliency based explanations need their own protection.
