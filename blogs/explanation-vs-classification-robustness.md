---
layout: page
permalink: /blogs/explanation-vs-classification-robustness/index.html
title: explanation-vs-classification-robustness
---

# Explanation Robustness and Classification Robustness Are Decoupled

*Paper "Are Classification Robustness and Explanation Robustness Really Strongly Correlated? An Analysis Through Input Loss Landscape" (SIGKDD Explorations).*

A common belief says that a model robust to adversarial attacks also has robust explanations, since both should come from a flat input loss landscape. This paper tests that belief and finds it does not hold. Improving how hard a saliency map is to attack does not flatten the landscape of the explanation loss, and flattening that landscape does not improve explanation robustness. The two robustness types are decoupled, which opens a real security gap, because an attacker can manipulate a saliency map without ever changing the predicted label.

![A saliency map manipulated while the prediction stays fixed](https://tiejin98.github.io/blog_image/explanation-vs-classification-robustness/overview.png)

*Adding crafted noise leaves the predicted label unchanged yet steers the saliency map toward a target map, which is the manipulation that motivates the study.*

**The insight.** Classification robustness does arise from a flat input loss landscape, but explanation robustness arises mainly from a high starting explanation loss, that is from how far and how hard the attack has to travel from its initial point. Flatness is the wrong lever for explanations. In fact flattening the explanation loss landscape lowers explanation robustness, while sharpening it raises explanation robustness, both at essentially the same adversarial accuracy.

## How the analysis works

1. Control classification robustness with TRADES, sweeping its trade-off weight to span a range of adversarial accuracy.
2. Make the evaluation tractable with cluster-based sampling, selecting prototypes from penultimate-layer features so explanation comparisons stay meaningful.
3. Measure explanation robustness by the explanation loss after an attack converges, and visualize the input loss landscape of that explanation loss.
4. Introduce SEP, which adds a regularizer on the explanation under random noise to a standard adversarial training loss. A positive weight flattens the landscape and a negative weight sharpens it.

## What the experiments show

Across MNIST, FMNIST, CIFAR10, CIFAR100, and TinyImageNet on both ConvNet and ResNet18, the sharpening variant always gives the strongest explanation robustness and the flattening variant always the weakest, while adversarial accuracy barely moves.

![Explanation and classification robustness move independently](https://tiejin98.github.io/blog_image/explanation-vs-classification-robustness/results.png)

On CIFAR10 with ResNet18 the explanation robustness varies by roughly four and a half times between the two SEP variants, yet adversarial accuracy stays near 29 percent for all of them. A Hessian analysis confirms the landscape effect.

## Takeaway

Robust classification does not buy robust explanations. Since the two can be moved independently, defending a model's predictions and defending its explanations are separate goals, and saliency-based explanations need their own protection.
