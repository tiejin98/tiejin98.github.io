---
layout: page
permalink: /blogs/syntrac-traffic-signal-dataset/index.html
title: syntrac-traffic-signal-dataset
---

# SynTraC: A Synthetic Dataset for Traffic Signal Control from Traffic Monitoring Cameras

Reinforcement learning controllers for traffic signals work well in simulation, but they learn from clean feature vectors such as per-lane vehicle counts, while real intersections only give camera images. This work introduces **SynTraC**, the first public dataset that pairs realistic intersection-camera images with traffic signal states and control rewards. You can read the full paper [here](https://arxiv.org/abs/2408.09588).

---

## The Challenge: Control Algorithms Learn from Counts, Not Cameras

Traditional signal control datasets hand the agent simplified counts from a 2D simulator, while vision datasets carry no signal or reward information. Nothing connects the realistic visual input a real deployment sees to the signal states and rewards a control algorithm needs, so methods cannot be studied on the input they will actually face.

---

## How SynTraC Is Built

SynTraC is rendered in a 3D simulator and ships over 86,000 images, more than 225,000 vehicle boxes, and over 250,000 reward values across varied weather and lighting.

![Overview of SynTraC across cameras, weather, and time of day](https://tiejin98.github.io/blog_image/syntrac-traffic-signal-dataset/overview.png)
*SynTraC provides synchronized views from four intersection cameras under varied weather and lighting, together with signal states and rewards that earlier datasets do not offer.*

The dataset is built in four steps.

1. Configure the scene by randomizing weather and time, setting twelve traffic flows at a four-way intersection, and placing four cameras that capture one frame per second.
2. Simulate and capture, grabbing one image per simulated second and projecting 3D positions into 2D bounding boxes and lane markers.
3. Assign lanes with a cross-product test between a lane reference vector and each vehicle vector.
4. Compute three rewards that control algorithms can optimize, namely waiting time, queue length, and throughput.

---

## Key Findings

The benchmark plugs a detection model that counts vehicles into a control policy and tests it online, revealing a clear gap between control with perfect counts and control that must read images.

![Control performance across weather and time](https://tiejin98.github.io/blog_image/syntrac-traffic-signal-dataset/results.png)

With ground-truth counts the learned policies beat the rule based baselines, but once a detection model supplies the counts, performance drops, and the drop is worst at night where detection error is largest.

---

## Takeaway

Image-based traffic signal control remains an open problem, since strong policies do not transfer cleanly once perception sits in the loop. SynTraC gives the community the images, signals, and rewards needed to attack that problem directly.
