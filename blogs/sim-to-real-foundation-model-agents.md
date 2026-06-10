---
layout: page
permalink: /blogs/sim-to-real-foundation-model-agents/index.html
title: sim-to-real-foundation-model-agents
---

# The Sim-to-Real Gap of Foundation Model Agents: A Unified MDP Perspective

A foundation model agent is a large language model that does more than chat, since it calls tools, reads the results, and takes actions to finish a task. These agents look strong on benchmarks and then stumble in deployment. This blue-sky paper argues that their failures are not a new mystery, because they mirror a problem reinforcement learning has studied for years, the sim-to-real gap, and it offers one framework to organize and fix them. You can read the full paper [here](https://arxiv.org/abs/2606.07017).

---

## The Challenge: Benchmarks Are Clean, Deployment Is Not

An agent is trained and scored on tidy benchmarks where the inputs are clean and a failure costs nothing. Deployment is the opposite, with noisy and multilingual requests, tools that time out or return malformed data, and real costs in money and latency. A high leaderboard score therefore says little about whether an agent will hold up in the wild, and the community keeps meeting these failures one at a time without a shared name for them.

---

## The Same Gap Robotics Already Studies

Reinforcement learning calls this exact problem the sim-to-real gap, the drop in performance when a policy trained in a simulator meets the messier real world. The bridge to agents is to describe both settings with one abstraction, a Markov decision process, which is just the loop of an agent making an observation, taking an action, the world transitioning to a new state, and a reward coming back. Those four parts, observation, action, transition, and reward, give four channels through which the gap can open, and naming them lets us borrow remedies that robotics already trusts rather than reinventing them.

---

## Four Channels, Four Familiar Fixes

The paper walks through each channel, the way it fails for an agent, and the established fix to carry over.

1. **Observation.** The agent's inputs drift through typos, paraphrases, or a switch of language, which is the perception mismatch robotics handles with domain randomization and adaptation.
2. **Action.** The set of available tools is crowded with near-duplicate or distracting options, answered by action shielding that filters unsafe or invalid calls.
3. **Transition.** Tools time out or return broken responses, the dynamics mismatch addressed by grounding methods that make a simulated tool behave like the real one.
4. **Reward.** A plain accuracy score ignores hidden fees and latency, which reward shaping and augmentation can fold back in.

To make one channel concrete, the observation gap shows up sharply in multilingual tool calling. A model often picks the right tool with the right intent, yet fills a parameter value in the user's language and breaks the tool's execution rules.

![Tool-calling error rates rise for non-English inputs](https://tiejin98.github.io/blog_image/sim-to-real-foundation-model-agents/results.png)
*Error rates rise once the request moves from English to other languages, and the same pattern shows up across every model tested.*

---

## A Research Agenda

Reading the gap through these four channels suggests a concrete program. Build stress-test benchmarks that inject controlled noise into each channel, rather than testing only on clean inputs. Share the perturbation recipes so results are comparable across groups, and design training that randomizes these conditions so robustness is learned rather than hoped for. The aim is one vocabulary and a shared way to ask whether an agent is ready to leave the simulator.

---

## Takeaway

Agent robustness already has a science behind it. Naming the failure as a sim-to-real gap over four channels gives the community one language, a set of ready remedies, and an honest way to measure trustworthiness before deployment.
