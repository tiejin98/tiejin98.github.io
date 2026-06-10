---
layout: page
permalink: /blogs/visshield-vlm-deidentification/index.html
title: visshield-vlm-deidentification
---

# Vision Language Model Helps Private Information De-Identification in Vision Data

Sensitive text often lives inside image pixels, for example protected health information printed on a scan or a form, and prior de-identification work mostly blurs faces and ignores this text. This work reframes the task as instruction-conditioned, selective OCR through a framework called **VisShield**. You can read the full paper [here](https://openreview.net/forum?id=g2CyeCHC32).

---

## The Challenge: Sensitive Text Hidden Inside Image Pixels

The only prior tool for textual de-identification in images cannot flexibly define what counts as private and performs poorly in practice. Two things are needed at once, namely accurately locating the sensitive text and then processing it for protection, and a rigid rule based tool cannot adapt to what each user or domain considers private.

---

## VisShield: De-Identification as Instruction-Conditioned OCR

The key idea is that a model which already does OCR can be taught to extract only the privacy relevant text, and because the definition of private information is part of the instruction, the same model adapts to new policies without redesign.

![The de-identification pipeline](https://tiejin98.github.io/blog_image/visshield-vlm-deidentification/overview.png)
*An instruction-tuned vision language model performs privacy-aware OCR, locating only the sensitive fields, then each detected box is masked by filling it with its corner pixel color.*

The method works in five steps.

1. Write instruction prompts with a precise definition of private information, a few in-context examples to handle format variation, a standardized OCR trigger token, and the generation step, covering eight categories such as name, SSN, address, and medical numbers.
2. Build synthetic images by overlaying realistic fake private information onto natural and medical base images with randomized fonts, sizes, and colors, which also yields exact ground-truth boxes.
3. Pair any prompt with any image, where the label contains only the boxes for the information types named in that prompt.
4. Fine-tune an OCR-capable model on this data, comparing full fine-tuning against a lightweight adapter.
5. At inference, output boxes for the defined private fields and mask each box.

---

## Key Findings

The fine-tuned model is evaluated across four image sources using F1 for OCR quality and IoU for localization.

![De-identification quality across information types and image sources](https://tiejin98.github.io/blog_image/visshield-vlm-deidentification/results.png)

Both adaptation strategies reach mean IoU above 0.9 and F1 around 0.93 to 0.99 across all categories and all image sets, while the rule based baseline collapses to near-zero localization and cannot match the text at all.

---

## Takeaway

Treating de-identification as instruction-driven selective OCR lets one fine-tuned vision language model both find and isolate sensitive text in images, with a flexibility and accuracy that fixed tools cannot reach.
