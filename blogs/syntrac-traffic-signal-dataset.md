---
layout: page
permalink: /blogs/syntrac-traffic-signal-dataset/index.html
title: syntrac-traffic-signal-dataset
---

# SynTraC: A Synthetic Dataset for Traffic Signal Control from Traffic Monitoring Cameras

Reinforcement learning, where a controller learns by trial and error from a reward signal, now drives strong traffic signal control in simulation. The catch is that these controllers learn from clean feature vectors such as per-lane vehicle counts, while a real intersection only offers camera images. This work introduces **SynTraC**, the first public image-based traffic signal control dataset, which pairs realistic intersection-camera images with traffic signal states and control rewards. You can read the full paper [here](https://arxiv.org/abs/2408.09588).

---

## Why Image-Based Signal Control Is Still Hard

Existing traffic-signal datasets fall into two camps that never meet. Some hand the controller simplified per-lane counts from a 2D simulator but no images, and others provide images but carry no signal state or reward. Because nothing connects the realistic visual input a deployed camera sees to the signals and rewards a controller needs, methods cannot be developed or fairly tested on the input they will actually face on the street. SynTraC is built to close that gap.

---

## Inside SynTraC

SynTraC is rendered in a 3D simulator and pairs a large collection of intersection images with the signal states and rewards a controller needs, all spanning varied weather and lighting.

![Overview of SynTraC across cameras, weather, and time of day](https://tiejin98.github.io/blog_image/syntrac-traffic-signal-dataset/overview.png)
*SynTraC provides synchronized views from four intersection cameras under varied weather and lighting, together with signal states and rewards that earlier datasets do not offer.*

**Diverse scenes come first.** Construction begins by randomizing weather and time of day and laying out many traffic flows at a four-way intersection watched by four cameras. The diversity is deliberate, because a controller that only ever sees clear daytime traffic will not survive rain or night, so the dataset bakes those conditions in from the start.

**Images arrive with exact labels.** The simulator is stepped forward and one frame per second is captured, and because the simulator knows every object's true position, those positions are projected into image-space bounding boxes and lane positions automatically. Generating data this way yields perfectly accurate annotations at a scale that would be costly and error prone to label by hand on real footage.

**Detections become the counts a controller expects.** Each vehicle is then assigned to a lane with a simple geometric test, which bridges raw detections to the per-lane counts that most signal policies consume. Finally SynTraC computes three reward signals, namely waiting time, queue length, and throughput, because a learning policy needs a reward and not just pictures, and supplying them is what turns a perception dataset into a control benchmark.

---

## Perception Is the Bottleneck, Especially at Night

When the policies are given perfect vehicle counts, most of the learned controllers outperform the rule based baselines, which confirms that the control side behaves as expected.

![Control performance across weather and time](https://tiejin98.github.io/blog_image/syntrac-traffic-signal-dataset/results.png)

The moment a real detection model supplies those counts, performance falls, and it falls the most at night where detection is hardest. This is the dataset's main message, since it shows that strong control does not survive the move from idealized counts to real perception, and that closing the gap is an open problem SynTraC is built to study.

---

## Takeaway

Image-based traffic signal control remains an open problem, since strong policies do not transfer cleanly once perception sits in the loop. SynTraC gives the community the images, signals, and rewards needed to attack that problem directly.
