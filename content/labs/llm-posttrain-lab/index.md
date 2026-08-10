---
title: "LLM Post-Training Lab"
description: "Experiments comparing SFT, DPO, PPO, and GRPO."
---

A controlled experimental setup for studying a question that is easy to state and surprisingly subtle in practice: **what changes when preference optimization is done with imitation learning, offline preference objectives, or reinforcement learning?**

The lab compares SFT, DPO, PPO, and GRPO from common starting policies across two tracks: helpfulness with learned reward models, and mathematical reasoning with verifiable rewards.

Rather than reporting a single score, the evaluation treats each checkpoint as a point on a frontier. It measures reward or task success against KL divergence from the reference policy, then adds diagnostics for capability retention, diversity and mode collapse, preference log-probabilities, and reward-model disagreement.

The implementation is designed as a reproducible research pipeline with smoke tests, resumable stages, parameter sweeps, incremental evaluation, and generated figures.

[Explore the repository on GitHub →](https://github.com/sinaghadermarzi/llm-posttrain-lab)
