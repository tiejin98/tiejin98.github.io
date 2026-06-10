---
layout: page
permalink: /blogs/syntrac-traffic-signal-dataset/index.html
title: syntrac-traffic-signal-dataset
---

# SynTraC. Teaching Traffic Signals to Read Camera Images

*Paper "SynTraC: A Synthetic Dataset for Traffic Signal Control from Traffic Monitoring Cameras" (IEEE ITSC). Code at https://github.com/DaRL-LibSignal/SynTraC*

Reinforcement learning controllers for traffic signals work well in simulation, but they learn from clean feature vectors such as per-lane vehicle counts. Real intersections only give camera images. SynTraC is the first public dataset that pairs realistic intersection-camera images with traffic signal states and reinforcement learning rewards, so control methods can finally be studied on the input that real deployments actually have.

**The gap it closes.** Traditional signal-control datasets hand the agent simplified counts from a 2D simulator, while vision datasets carry no signal or reward information. SynTraC bridges the two. It is rendered in the CARLA 3D simulator and ships over 86,000 RGB images, more than 225,000 vehicle bounding boxes, and over 250,000 reward values, spanning sunny, rainy, foggy, cloudy weather and both day and night.

![Overview of SynTraC across cameras, weather, and time of day](https://tiejin98.github.io/blog_image/syntrac-traffic-signal-dataset/overview.png)

*SynTraC provides synchronized views from four intersection cameras under varied weather and lighting, together with signal states and rewards that earlier datasets do not offer.*

## How the dataset is built

1. Configure the scene. Randomize weather and time, set twelve traffic flows at a four-way intersection, and place four 1080p cameras that capture one frame per second.
2. Simulate and capture. Run CARLA in fixed-time synchronous mode, grab one image per simulated second, and use the camera projection matrix to turn 3D positions into 2D bounding boxes and lane markers.
3. Assign lanes. Use a cross-product test between a lane reference vector and each vehicle vector to label which lane a car belongs to.
4. Compute rewards. Produce three signals that control algorithms can optimize, namely waiting time, queue length, and throughput.

The benchmark then plugs a detection model that counts vehicles per lane into a control policy, and tests it online inside CARLA. Detection models include Faster R-CNN and RetinaNet, and controllers include Fix-Time, MaxPressure, DQN, DDQN, SAC, and CQL.

## What the experiments show

The headline result is a clear gap between control that uses perfect counts and control that must read images.

![Control performance across weather and time](https://tiejin98.github.io/blog_image/syntrac-traffic-signal-dataset/results.png)

With ground-truth counts, CQL is best across every metric and the learned policies beat the rule-based baselines. Once a detection model supplies the counts, performance drops, and the drop is worst at night where detection error is largest. For example CQL with Faster R-CNN sees waiting time climb from 1439 with perfect counts to 1918 even in the easiest sunny daytime setting, and higher still at night.

## Takeaway

Image-based traffic signal control remains an open problem. Strong reinforcement learning policies do not transfer cleanly once perception sits in the loop, and nighttime perception is the main bottleneck. SynTraC gives the community the images, signals, and rewards needed to attack that problem directly.
