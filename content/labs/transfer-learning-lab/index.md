---
title: "Transfer Learning Lab"
description: "Small experiments on what transfers between neural-network tasks — and what does not."
---

A CPU-friendly set of experiments on transferability between tasks in neural networks.

The lab uses controlled MNIST and Fashion-MNIST variants with a shared CNN so each experiment can compare directly against a from-scratch baseline. It studies pretraining and fine-tuning, layer transferability, task-transfer matrices, negative transfer, catastrophic forgetting, multi-task synergy and interference, modular composition, and self-supervised pretext tasks.

A key goal is to make broad transfer-learning claims concrete enough to falsify. The executed notebooks include machine-checked claim assertions and experiments such as asymmetric task transfer, poisoned and frozen representations, replay against forgetting, gradient conflict between auxiliary tasks, and a pretrained encoder reused inside a compound two-digit architecture.

[Explore the repository on GitHub →](https://github.com/sinaghadermarzi/transfer-learning-lab)
