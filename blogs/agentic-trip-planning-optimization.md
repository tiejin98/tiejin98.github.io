---
layout: page
permalink: /blogs/agentic-trip-planning-optimization/index.html
title: agentic-trip-planning-optimization
---

# Agentic AI for Trip Planning that Optimizes, Not Just Plans

*Paper "Agentic AI for Trip Planning Optimization Application" (IEEE IV 2026). Work done during an internship at Toyota InfoTech Labs.*

Most LLM trip planners aim for a feasible itinerary. For an intelligent vehicle that is not enough, because travel time, traffic, charging, and dwell time all interact, and the goal should be the optimal route rather than any valid one. This paper contributes two things, an agentic system that searches for optimal plans and a benchmark with definitive ground-truth answers so optimization can actually be scored.

**The insight.** Multi-agent superiority is not automatic. What makes it work is centralized orchestration plus the ability to re-think. A single controller that decomposes the task, dispatches specialized agents, checks consistency, and self-corrects clearly beats both a single agent and a decentralized swarm. Without orchestration a swarm can even fall behind one well-prompted agent.

![The three-component agentic system](https://tiejin98.github.io/blog_image/agentic-trip-planning-optimization/overview.png)

*An Orchestration Agent sits between the in-vehicle interaction layer and a pool of specialized agents for traffic, calculation, and points of interest, and it re-thinks whenever a sub-task fails.*

## How the system works

1. An In-Vehicle Agent turns the natural-language request into a clear structured query and handles clarification.
2. The Orchestration Agent decomposes the goal into interdependent sub-tasks and dispatches them.
3. Specialized agents execute. A Traffic Agent returns time-conditioned travel times, a Calculation Agent scores itineraries and spots concurrent savings such as charging during a coffee stop, and POI agents act as digital twins that return dwell time, hours, and cost.
4. The Orchestrator integrates results and re-thinks on errors, so the workflow is dynamic rather than a fixed chain.

## Building a benchmark with real ground truth

Existing benchmarks give reference answers without true optima, which forces biased LLM-as-judge scoring. The TOP benchmark instead computes exact optimal solutions. Fifty POIs are collected with all pairwise travel times queried at four times of day, 500 questions are generated across 15 categories and three difficulty levels, and every question carries a deterministic workflow that produces the verifiable optimal answer.

## What the experiments show

All systems use GPT-4o, the same tools, and the same offline database, and a response counts only on an exact match to the optimum.

![Accuracy across difficulty levels on TOP](https://tiejin98.github.io/blog_image/agentic-trip-planning-optimization/results.png)

The agentic system reaches 77.4 percent overall, far above the single agent at 30.4 percent and the swarm at 23.6 percent. The gap widens with difficulty, reaching 58 percent on hard questions where both baselines stay near 9 percent. Notably the single agent beats the swarm, which confirms that orchestration, not agent count, drives the gains.

## Takeaway

Optimization-grade trip planning needs a controller that reasons about the whole plan and recovers from its own mistakes, and it needs a benchmark with true optima to measure progress. This paper delivers both.
