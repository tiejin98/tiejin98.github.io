---
layout: page
permalink: /blogs/sim-to-real-foundation-model-agents/index.html
title: sim-to-real-foundation-model-agents
---

# The Sim-to-Real Gap of Foundation Model Agents: A Unified MDP Perspective

Foundation model agents look strong on benchmarks and then break in deployment. This work argues that these failures are not new, since they mirror the well studied sim-to-real gap of reinforcement learning, and proposes one unified view that lets mature remedies be reused. You can read the full paper [here](https://arxiv.org/abs/2606.07017).

---

## The Challenge: Benchmarks Are Clean, Deployment Is Not

Agents are trained on curated benchmarks where data is abundant and failures are safe, then deployed into noisy and multilingual inputs, unreliable tools, flaky APIs, and real cost and latency. Leaderboard performance therefore does not equal real-world reliability, and the community keeps rediscovering these gaps without a shared language for them.

![The sim-to-real gap as a wall across four channels](https://tiejin98.github.io/blog_image/sim-to-real-foundation-model-agents/overview.png)
*A policy trained in a simulated environment is deployed unchanged in the real one, and performance leaks through a wall built from four channels, observation, action, transition, and reward.*

---

## A Unified View Across Four MDP Channels

The proposal is to model any agent as acting in a Markov decision process and attribute the performance drop to four channels, each with a ready remedy from reinforcement learning and robotics.

1. Observation, covering typos, paraphrases, and multilingual inputs, addressed by domain randomization and adaptation.
2. Action, covering overlapping or distractor tools, addressed by action shielding and delay aware control.
3. Transition, covering timeouts and malformed responses, addressed by grounded action transformation and robust learning.
4. Reward, covering hidden fees and latency that a binary accuracy reward ignores, addressed by reward shaping and augmentation.

---

## Evidence

The paper grounds the observation channel with a concrete multilingual tool calling study, where models pick the right tool yet fill parameter values in the user's language and break execution conventions.

![Tool-calling error rates rise sharply for non-English inputs](https://tiejin98.github.io/blog_image/sim-to-real-foundation-model-agents/results.png)

Error rates jump severalfold when the instruction moves from English to other languages, and the same pattern holds across models, which shows the gap is structural rather than a single model's quirk.

---

## Takeaway

Agent robustness already has a science behind it. Naming the foundation model agent problem as a sim-to-real gap over four channels gives the community one vocabulary, a set of ready remedies, and a clear way to measure trustworthiness before deployment.
