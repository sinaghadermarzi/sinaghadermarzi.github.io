---
title: "LLM Text Generation Lab"
date: 2026-07-19
description: "Decoding and inference-time computation, from provider controls to local generation loops."
---

An experiment-first lab for understanding how language models generate text and how inference-time computation changes results.

One track compares provider-exposed reasoning and sampling controls across hosted APIs. The other uses local Hugging Face models to inspect and modify the decoding loop directly: logits, greedy decoding, sampling, beam search, stopping criteria, and repetition controls.

The later experiments move beyond one-shot decoding into multiple candidates, self-consistency, verifier and critic loops, adaptive compute budgets, and model-agnostic search including Monte Carlo tree search. Measurements include task score, output length, latency, calls, cost, and provider-reported reasoning usage where available.

[Explore the repository on GitHub →](https://github.com/sinaghadermarzi/llm-textgen-lab)
