---
layout: page
permalink: /blogs/mm-privacy-multimodal-leakage/index.html
title: mm-privacy-multimodal-leakage
---

# MM-Privacy. Privacy Leaks in Multi-modal LLMs Depend on the Task

*Paper "Unveiling Privacy Risks in Multi-modal Large Language Models: Task-specific Vulnerabilities and Mitigation Challenges" (ACL 2025). Code at https://github.com/tiejin98/Privacy_Different_Task*

Text-only LLMs are known to memorize and leak personal data. Multi-modal LLMs add a new surface, since they can read sensitive information printed in an image or recall it from fine-tuning. MM-Privacy is a benchmark of more than 13,000 samples that measures this leakage, and its main finding is uncomfortable. Whether a model leaks depends heavily on how you ask, and existing safeguards do not generalize across ways of asking.

**The insight.** For aligned closed-source models, indirect tasks such as captioning and rephrasing leak the most, because they pull attention away from privacy, much like a jailbreak, while a direct request is refused. For open-source models the opposite holds, since weak alignment means asking directly works best and harder tasks like captioning overwhelm smaller models. Privacy mitigation therefore has to be task-aware.

![Scenarios and prompts in the MM-Privacy benchmark](https://tiejin98.github.io/blog_image/mm-privacy-multimodal-leakage/overview.png)

*The benchmark spans four realistic scenarios, hiring, verification, finance, and open context, with forms that embed synthetic emails, phone numbers, and SSNs.*

## How the benchmark is built

1. Define two risks. Disclosure risk is leaking information that sits in the input image. Retention risk is leaking information memorized during fine-tuning.
2. Generate synthetic private information with Faker, then embed it into form images, made by automatic templates, by humans filling and photographing printed forms, and by Stable Diffusion with a human quality filter.
3. Separate a memory set, injected only for the retention test, from a non-overlapping evaluation set, so any leak is attributable to memorization rather than to reading the input.
4. Probe each item under five tasks, Directly Ask, Captioning, VQA, Rephrasing, and Classification, after prepending a system prompt that offers a refusal option.
5. Score with Attack Success Rate, where higher means more leakage, and Refuse Rate, where lower means more leakage.

## What the experiments show

The results expose both the open-versus-closed gap and the task inconsistency.

![Leakage across tasks and models for open-source MLLMs](https://tiejin98.github.io/blog_image/mm-privacy-multimodal-leakage/results.png)

Open-source models leak heavily, and Idefics2 under supervised fine-tuning is the worst, reaching ASR around 0.84 for email and phone and 0.76 for SSN when asked directly. Closed-source models flip the pattern, where GPT-4o refuses a direct SSN request yet leaks most under captioning. Among injection methods, supervised fine-tuning overfits and leaks most, while contrastive learning preserves the most privacy.

## Takeaway

Privacy in multi-modal LLMs is not a single property of a model. The same image leaks different amounts depending on the task framing, so any real defense must hold across direct and indirect requests, not just block the obvious question.
