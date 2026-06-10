---
layout: page
permalink: /blogs/mm-privacy-multimodal-leakage/index.html
title: mm-privacy-multimodal-leakage
---

# Unveiling Privacy Risks in Multi-modal Large Language Models: Task-specific Vulnerabilities and Mitigation Challenges

Multi-modal large language models, which read both text and images, can leak sensitive information printed in an image or recall it from fine-tuning, yet this risk is underexplored. This work introduces **MM-Privacy**, a benchmark that measures such leakage and shows it depends heavily on how the model is asked. You can read the full paper [here](https://openreview.net/forum?id=wXWfOThQCT).

---

## Leakage That Depends on How You Ask

Privacy leakage in text-only models is well documented, but multi-modal models add new risks that prior work barely touched, since earlier studies mostly asked whether a model can recognize private information rather than whether it leaks it. The headline finding is that leakage is task inconsistent, so a safeguard tuned for one way of asking does not carry over to another. Aligned closed-source models tend to refuse a blunt request, yet indirect tasks such as captioning and rephrasing slip past their guardrails more often. Open-source models behave almost in reverse, since weaker alignment makes a direct question the most effective probe.

![Scenarios and prompts in the benchmark](https://tiejin98.github.io/blog_image/mm-privacy-multimodal-leakage/overview.png)
*The benchmark spans four realistic scenarios, hiring, verification, finance, and open context, with forms that embed synthetic emails, phone numbers, and social security numbers.*

---

## Building MM-Privacy

To pin down such a slippery effect, the benchmark is built so every source of leakage can be attributed cleanly.

### Two kinds of risk

It separates disclosure risk, leaking information that sits in the input image, from retention risk, leaking information memorized during fine-tuning. These are different threat models, so measuring them apart is what lets the study say where a leak actually came from.

### Realistic images from several sources

Synthetic personal information is embedded into form images built three ways, by automatic templates, by people who fill in and photograph printed forms, and by a diffusion model whose scenes pass through a human quality filter, with the diffusion images reserved for the memorization test. Several sources at once buy both scale and realism, and keeping every record synthetic keeps the benchmark ethical.

### Memory kept apart from evaluation

The information injected into the model's memory and the information used to probe it come from non-overlapping sets. This separation is essential, because it ensures a leak in the retention test reflects genuine memorization rather than the model simply reading its input.

### Five framings of the same request

Every item is posed five ways, namely directly ask, captioning, visual question answering, rephrasing, and classification, then scored by attack success rate, how often the private value is leaked, together with refuse rate, how often the model declines. Asking the same thing in different framings is the heart of the design, since it is exactly how the study reveals that leakage tracks the framing rather than the content.

---

## The Same Image Leaks Differently Depending on the Ask

Open-source models leak heavily and often comply outright, while closed-source models show a subtler pattern, since indirect framings like captioning and rephrasing bypass their safeguards more often than a direct request does.

![Leakage across tasks and models](https://tiejin98.github.io/blog_image/mm-privacy-multimodal-leakage/results.png)

How the information is injected matters too, since straightforward fine-tuning memorizes and leaks the most while a contrastive objective protects it best. Taken together the results support the central claim that privacy risk here is task dependent, so a safeguard that blocks the obvious question can still be bypassed simply by rephrasing it.

---

## Takeaway

Privacy in multi-modal models is not a single property of a model. The same image leaks different amounts depending on the task framing, so a real defense must hold across direct and indirect requests, not just block the obvious question.
