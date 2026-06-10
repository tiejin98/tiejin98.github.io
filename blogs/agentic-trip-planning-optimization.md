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

The central insight is that multi-agent superiority is not automatic, and what makes it work is a central controller paired with the ability to re-think and self-correct.

![The three-component agentic system](https://tiejin98.github.io/blog_image/agentic-trip-planning-optimization/overview.png)
*An Orchestration Agent sits between the in-vehicle interaction layer and a pool of specialized agents for traffic, calculation, and points of interest, and it re-thinks whenever a sub-task fails.*

### Ground the request before planning

An In-Vehicle Agent first turns the driver's natural language request into a clear structured query and resolves anything ambiguous. The motivation is simple, since optimization is meaningless if the goal is misread, so intent is pinned down before any search begins.

### Let one controller own the whole plan

The Orchestration Agent decomposes the goal into interdependent sub-tasks and decides which specialist to call next. A single pass cannot optimize a route where each choice depends on the others, so a central controller is needed to hold the entire plan in view and weigh trade-offs across sub-tasks rather than greedily.

### Delegate to specialists with the right tools

Execution is handed to focused agents, a Traffic Agent for time conditioned travel times, a Calculation Agent that scores itineraries and spots concurrent savings such as charging during a coffee stop, and points-of-interest agents that report dwell time, hours, and cost. Each task needs its own data and tools, so splitting the work keeps every piece accurate and lets the controller combine parts it can trust.

### Re-think when a step fails

Crucially, the Orchestrator inspects what comes back and re-issues a corrected request when something is wrong, so the workflow is dynamic rather than a fixed chain. This is exactly what separates it from a decentralized swarm, which tends to loop until timeout when a sub-task errs instead of diagnosing and repairing the mistake.

---

## A Benchmark with Ground-Truth Optima

Because existing benchmarks lack a true optimum, the work also builds TOP, which attaches a deterministic workflow to every question that computes the exact best plan, so an answer can be checked against ground truth instead of a biased judge. It spans many question categories and three difficulty levels, which makes it possible to see not just whether a system is right but where its optimization starts to break down.

---

## Orchestration, Not Agent Count, Drives the Gains

Holding the model, tools, and database fixed, the orchestrated system is far more accurate than both a single agent and a decentralized swarm, and its lead grows as questions get harder and force more constraints to be juggled at once.

![Accuracy across difficulty levels](https://tiejin98.github.io/blog_image/agentic-trip-planning-optimization/results.png)

The most telling result is that the single agent actually beats the swarm, which makes the point sharply, since simply adding agents without a controller hurts rather than helps. The advantage comes from coordination and self-correction, precisely the pieces the swarm lacks.

---

## Takeaway

Optimization grade trip planning needs a controller that reasons about the whole plan and recovers from its own mistakes, and it needs a benchmark with true optima to measure progress. This work delivers both.
