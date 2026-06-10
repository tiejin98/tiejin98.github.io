---
layout: page
permalink: /blogs/agentic-trip-planning-optimization/index.html
title: agentic-trip-planning-optimization
---

# Agentic AI for Trip Planning Optimization Application

Most LLM trip planners aim for a feasible itinerary, which is not enough for an intelligent vehicle where travel time, traffic, charging, and dwell time all interact and the goal should be an optimal route. This work contributes an agentic system that searches for optimal plans together with a benchmark whose answers carry definitive ground truth optima. You can read the full paper [here](https://arxiv.org/abs/2605.00276).

---

## The Challenge: Feasible Plans Are Not Optimal Plans

Classical pathfinders ignore preferences, manual planning is suboptimal, and existing LLM systems target feasibility rather than optimality. Evaluation is just as hard, because prior benchmarks give reference answers without true optima, which forces biased model-as-judge scoring and hides where optimization actually fails.

---

## A Coordinated Multi-Agent System

The central insight is that multi-agent superiority is not automatic, and what makes it work is centralized orchestration plus the ability to re-think and self-correct.

![The three-component agentic system](https://tiejin98.github.io/blog_image/agentic-trip-planning-optimization/overview.png)
*An Orchestration Agent sits between the in-vehicle interaction layer and a pool of specialized agents for traffic, calculation, and points of interest, and it re-thinks whenever a sub-task fails.*

The system works in four parts.

1. An In-Vehicle Agent turns the natural language request into a clear structured query and handles clarification.
2. The Orchestration Agent decomposes the goal into interdependent sub-tasks and dispatches them.
3. Specialized agents execute, where a Traffic Agent returns time conditioned travel times, a Calculation Agent scores itineraries and spots concurrent savings such as charging during a coffee stop, and points-of-interest agents return dwell time, hours, and cost.
4. The Orchestrator integrates the results and re-thinks on errors, so the workflow is dynamic rather than a fixed chain.

To measure optimization rather than feasibility, the TOP benchmark computes exact optimal solutions for 500 questions across 15 categories and three difficulty levels, where every question carries a deterministic workflow that produces the verifiable answer.

---

## Key Findings

All systems use the same model, tools, and offline database, and a response counts only on an exact match to the optimum.

![Accuracy across difficulty levels](https://tiejin98.github.io/blog_image/agentic-trip-planning-optimization/results.png)

The agentic system reaches 77.4 percent overall, far above the single agent at 30.4 percent and the swarm at 23.6 percent, and the gap widens to nearly seven times on hard questions. Notably the single agent beats the swarm, which confirms that orchestration, not agent count, drives the gains.

---

## Takeaway

Optimization grade trip planning needs a controller that reasons about the whole plan and recovers from its own mistakes, and it needs a benchmark with true optima to measure progress. This work delivers both.
