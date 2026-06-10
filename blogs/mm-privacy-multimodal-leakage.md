---
layout: page
permalink: /blogs/mm-privacy-multimodal-leakage/index.html
title: mm-privacy-multimodal-leakage
---

# Unveiling Privacy Risks in Multi-modal Large Language Models: Task-specific Vulnerabilities and Mitigation Challenges

Multi-modal LLMs can read sensitive information printed in an image or recall it from fine-tuning, yet this risk is underexplored. This work introduces **MM-Privacy**, a benchmark that measures such leakage and shows it depends heavily on how the model is asked. You can read the full paper [here](https://openreview.net/forum?id=wXWfOThQCT).

---

## The Challenge: Privacy Leakage Depends on How You Ask

Privacy risk in multi-modal LLMs is task inconsistent, and existing safeguards do not generalize across ways of asking. For aligned closed-source models, indirect tasks such as captioning and rephrasing leak the most because they pull attention away from privacy, while a direct request is refused. For open-source models the opposite holds, since weak alignment means asking directly works best. Any real defense therefore has to be task aware.

![Scenarios and prompts in the benchmark](https://tiejin98.github.io/blog_image/mm-privacy-multimodal-leakage/overview.png)
*The benchmark spans four realistic scenarios, hiring, verification, finance, and open context, with forms that embed synthetic emails, phone numbers, and SSNs.*

---

## Building the MM-Privacy Benchmark

The benchmark is constructed in five steps.

1. Define two risk types, disclosure risk for information present in the input image and retention risk for information memorized during fine-tuning.
2. Generate synthetic private information and embed it into form images, made by automatic templates, by humans filling printed forms, and by a diffusion model with a human quality filter.
3. Separate a memory set, injected only for the retention test, from a non-overlapping evaluation set, so any leak is attributable to memorization rather than to reading the input.
4. Probe each item under five tasks, namely directly ask, captioning, visual question answering, rephrasing, and classification.
5. Score with attack success rate, where higher means more leakage, and refuse rate, where lower means more leakage.

---

## Key Findings

The results expose both the open versus closed gap and the task inconsistency.

![Leakage across tasks and models](https://tiejin98.github.io/blog_image/mm-privacy-multimodal-leakage/results.png)

Open-source models leak heavily, and one model under supervised fine-tuning reaches attack success around 0.84 for email and phone when asked directly. Closed-source models flip the pattern, refusing a direct request yet leaking most under captioning. Among injection methods, supervised fine-tuning overfits and leaks most, while contrastive learning preserves the most privacy.

---

## Takeaway

Privacy in multi-modal LLMs is not a single property of a model. The same image leaks different amounts depending on the task framing, so a real defense must hold across direct and indirect requests, not just block the obvious question.
