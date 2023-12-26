---
title: "Youtube's Watch Next Recommendations"
date: 2023-12-25T16:01:27+05:30
draft: true
categories:
    - machine learning
    - recsys
    - deep learning
    - scale
tags:
    - machine learning
    - recsys
    - deep learning
    - scale
---

These are my notes on the paper [Recommending What Video to Watch Next: A Multitask Ranking System](https://daiwk.github.io/assets/youtube-multitask.pdf), additionally some code that I feel like writing.

### Learning Objective

The abstract of this paper suggests that the recommendation network Youtube trains for this problem is optimised against multiple learning/ranking objectives which compete with each other and are therefore not complementary to each other.

* The way they expect to achieve this is via "soft parameter sharing" specifically a multi-gate MoE which they claim is efficient for parameter sharing.
* They will also present a wide and deep framework that eliminates implicit selection biases in user feedback (which they claim exist in their data).


### Motivating the Problem

