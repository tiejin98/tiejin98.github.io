---
layout: page
permalink: /blogs/visshield-vlm-deidentification/index.html
title: visshield-vlm-deidentification
---

# Vision Language Models Can De-Identify Private Text in Images

*Paper "Vision Language Model Helps Private Information De-Identification in Vision Data" (ACL 2025).*

Sensitive text often lives inside image pixels, for example protected health information printed on a scan or a form. Prior de-identification work mostly blurs faces and ignores this text, and the one available tool for textual de-identification in images cannot adapt to what each user considers private. This paper reframes the task as instruction-conditioned, selective OCR. A vision language model reads only the text that matches a context-specific privacy definition and returns its bounding boxes, which are then masked.

**The insight.** A model that already does OCR can be taught to extract just the privacy-relevant text rather than all text. Because the definition of private information is part of the instruction, the same model adapts to medical records, hiring forms, or any new policy without redesign, which rigid rule-based tools cannot do.

![The de-identification pipeline](https://tiejin98.github.io/blog_image/visshield-vlm-deidentification/overview.png)

*An instruction-tuned vision language model performs privacy-aware OCR, locating only the sensitive fields, then each detected box is masked by filling it with its corner pixel color.*

## How it works

1. Write instruction prompts with four parts, a precise definition of private information, a few in-context examples to handle format variation, a standardized OCR trigger token, and the generation step. The prompts cover eight categories such as name, SSN, address, email, and medical numbers.
2. Build synthetic images by overlaying fake but realistic private information, generated with Faker, onto natural and medical base images using randomized fonts, sizes, and colors. The renderer also yields exact ground-truth bounding boxes, giving 20,000 images and more than 130,000 boxes.
3. Pair any prompt with any image. The label contains only the boxes for the information types named in that prompt, or a no-private-information response when none apply.
4. Fine-tune the OCR-capable Kosmos-2.5 on this data, comparing full fine-tuning against LoRA. Only 100,000 samples are needed for strong, generalizable behavior.
5. At inference, the model outputs boxes for the defined private fields, and each box is masked.

## What the experiments show

The fine-tuned model is evaluated across four image sources with F1 for OCR quality and IoU for localization.

![De-identification quality across information types and image sources](https://tiejin98.github.io/blog_image/visshield-vlm-deidentification/results.png)

Both full fine-tuning and LoRA reach mean IoU above 0.9 and F1 around 0.93 to 0.99 across all eight categories and all four image sets. The Presidio baseline collapses to near-zero IoU and cannot match the text at all. The end-to-end model also clearly beats a training-free pipeline that chains Tesseract with an LLM.

## Takeaway

Treating de-identification as instruction-driven selective OCR lets one fine-tuned vision language model both find and isolate sensitive text in images, with a flexibility and accuracy that fixed tools cannot reach.
