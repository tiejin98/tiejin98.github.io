---
layout: page
permalink: /blogs/sim-to-real-foundation-model-agents/index.html
title: sim-to-real-foundation-model-agents
---

# The Sim-to-Real Gap of Foundation Model Agents

*Blue Sky paper "The Sim-to-Real Gap of Foundation Model Agents: A Unified MDP Perspective" (KDD 2026).*

Foundation model agents look strong on benchmarks and then break in deployment. They face noisy and multilingual inputs, unreliable tools, flaky APIs, and real cost and latency. This paper makes a simple but bold claim. These failures are not new. They are the same sim-to-real gap that reinforcement learning and robotics have studied for years, and the foundation model community is rediscovering it without a shared language.

**The position.** Model any agent as acting in a Markov decision process with observation, action, transition, and reward. Define the sim-to-real gap as the performance drop between the simulated training environment and the real deployment environment, then attribute that drop to discrepancies in each of the four channels. Once the problem is framed this way, mature remedies such as domain randomization and grounded action transformation can be ported directly instead of reinvented.

![The sim-to-real gap as a wall across four MDP channels](https://tiejin98.github.io/blog_image/sim-to-real-foundation-model-agents/overview.png)

*A policy trained in a simulated MDP is deployed unchanged in the real MDP, and performance leaks through a wall built from four channels, observation, action, transition, and reward.*

## The four channels and their fixes

1. Observation. Typos, paraphrases, multilingual inputs, and multi-modal noise. Borrow domain randomization, domain adaptation, and sensor fusion.
2. Action. Overlapping or distractor tools and noisy execution. Borrow action shielding and delay-aware control.
3. Transition. Timeouts, partial API failures, and malformed responses. Borrow grounded action transformation and distributionally robust learning.
4. Reward. Hidden fees, latency, and misleading tool names that a binary accuracy reward ignores. Borrow reward shaping and augmentation.

## Evidence that the gap is real

The paper grounds the observation channel with a concrete multilingual tool-calling study. Models often pick the right tool with the right intent, yet fill parameter values in the user's language and break English-centric execution conventions.

![Tool-calling error rates rise sharply for non-English inputs](https://tiejin98.github.io/blog_image/sim-to-real-foundation-model-agents/results.png)

Error rates jump from 13.5 percent to 28.5 percent for GPT-5 and from 5.5 percent to 46.5 percent for Qwen3-Next-80B when the instruction moves from English to Chinese. The same pattern holds across models and languages, which shows the gap is structural rather than a single model's quirk.

## A research agenda

The authors call for standardized stress-test benchmarks that inject controlled perturbations into each channel, built on top of existing suites, plus shared perturbation libraries and per-gap leaderboards. They also ask whether larger models close these gaps on their own, and how the channels interact, since noisy observations often arrive together with delayed transitions.

## Takeaway

Agent robustness already has a science behind it. Naming the foundation model agent problem as a sim-to-real gap over four MDP channels gives the community one vocabulary, a set of ready remedies, and a clear way to measure trustworthiness before deployment.
