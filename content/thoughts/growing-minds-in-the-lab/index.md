---
title: "Growing Minds in the Lab"
# draft: true
date: 2026-08-23
description: "A preregistered experiment — one base model, eight persistent architectures, months of shared tasks — that someone could start this month."
---

This experiment could start this month. Not "in principle," not "once the field matures" — the benchmarks exist, the harnesses exist, the architectures are all published, and the compute is a rounding error next to a pretraining run. What is missing is only the decision to run it, and the discipline to design it so its results mean something.

The claim it would test is the one I have been building toward across this series: that [identical base models can become different minds](/thoughts/same-model-different-minds/) through the architecture of their accumulated external cognition, and that [what one architecture produces can be differentially valuable to another](/thoughts/artifact-compatibility/). I have looked for a published version of this experiment and cannot find one. Nothing holds the base model and compute fixed, varies only the persistent architecture, and measures long-horizon divergence. The nearest cousin, [Multiagent Finetuning](https://arxiv.org/abs/2501.05707), diverges copies of the same base model into generator and critic specialists — but through their *weights*. The artifact version is unrun. That absence is the opportunity.

## One model, eight upbringings

Take one frozen base model and one shared task stream — research questions, coding tasks, planning problems, arriving over months. Give each instance the same compute budget and a different persistent architecture:

```text
A. Markdown + hierarchical folders     E. episodic provenance log
B. Markdown + semantic search          F. simulator / state store
C. code and tool library               G. formal propositions + verifier
D. knowledge graph                     H. hybrid
```

Then let them live. Not for a benchmark session — for a long horizon, because the early evidence says horizon is where architecture reveals itself. One July preprint reports that memory-architecture rankings *invert* with history length: a curated-map memory that led at three weeks (96% recall) fell to 72% by nine weeks while a provenance-typed graph rose to about 90% ([arXiv:2607.21962](https://arxiv.org/abs/2607.21962)) — though I must flag that this is a weeks-old, unreplicated preprint on fully synthetic "life-script" data, a first observation rather than an established phenomenon. In the same spirit, the [filesystem-memory paper](https://arxiv.org/abs/2607.26637) — also fresh and also unreplicated — finds that self-maintained organization *erodes* over time for all but the strongest management agents. Short evaluations cannot see any of this. If minds differentiate through accumulated structure, they differentiate slowly, and the experiment has to be long enough to watch it happen.

The infrastructure for long watching already exists. [Vending-Bench](https://arxiv.org/abs/2502.15840) runs agents through 20-million-token business simulations and catches catastrophic long-horizon failures unrelated to context limits. [StreamBench](https://arxiv.org/abs/2406.08747) evaluates improvement over a task stream. [MemoryAgentBench](https://arxiv.org/abs/2507.05257) feeds context incrementally and scores four separate memory competencies. [LongMemEval](https://arxiv.org/abs/2410.10813) provides freely scalable interaction histories. Nobody needs to build the lab from scratch; they need to point it at a factorial design.

## The controls that keep it honest

Here is where most versions of this experiment would die on contact with reviewers, because the multi-agent literature has a well-earned deflationary streak. Simple sampling-and-voting with copies of one model reproduces much of the gain attributed to elaborate agent societies ([More Agents Is All You Need](https://arxiv.org/abs/2402.05120)); voting performance is [non-monotonic in the number of calls](https://arxiv.org/abs/2403.02419); and aggregating multiple samples of the single best model has been reported to beat mixing different models ([Self-MoA](https://arxiv.org/abs/2502.00674)). Any "diversity gain" that lacks compute-matched controls will be dismissed — correctly — as test-time compute in a costume.

So the design needs four baselines at exactly equal token budget: a single agent with no persistent architecture; the same agent with self-consistency sampling; a Self-MoA-style aggregation of the best single configuration; and a full-context baseline that simply carries the whole history, since on small histories full context is already a strong quality baseline. The heterogeneous society earns its thesis only by beating all four. And when societies fail, the failure taxonomy matters: [MAST](https://arxiv.org/abs/2503.13657) attributes most multi-agent failures to organizational causes rather than model limits, which is precisely why coupling design should be measured rather than assumed.

## Preregister the endpoints, and the kill conditions

Three endpoint families, written down before the first run.

**Divergence.** Probe every instance on a held-out battery at regular intervals and track:

$$
D(t) = \frac{\mathrm{Var}_{\text{between}}\big(b_1(t), \ldots, b_8(t)\big)}{\mathrm{Var}_{\text{within}}(t)}
$$

In words: divergence is real only when the behavioral variance *between* architectures grows over time and exceeds the run-to-run variance *within* replicates of a single architecture. The thesis needs the numerator to grow with horizon; noise has no such obligation.

**Per-workload Pareto positions.** Not a leaderboard — a frontier. Each architecture gets a coordinate per workload: accuracy per token per unit latency, plus write and maintenance cost. The prediction from the workload-dependence evidence (different memory structures already win different tasks in [controlled comparisons](https://arxiv.org/abs/2412.15266)) is that no architecture dominates; architectures should occupy different frontier positions, not one point.

**The C_ij matrix, diagonal included.** Feed architecture j the persistent artifacts produced by architecture i and measure the performance change — the [artifact-compatibility](/thoughts/artifact-compatibility/) experiment. The diagonal matters: each architecture consuming its own artifacts is the null against which every off-diagonal cell is judged. Without it, cross-architecture "synergies" are unfalsifiable.

And the kill conditions, stated in advance: the divergence thesis is refuted if D(t) plateaus into noise; the [ecology thesis](/thoughts/epistemic-ecology/) is refuted if the best homogeneous society — including the Self-MoA control — matches every heterogeneous one at equal total compute; the compatibility thesis is refuted if the off-diagonal structure of C_ij is statistically indistinguishable from its diagonal.

## The instrument is the hard part

There is one hazard that could sink the whole program, and it is not compute. It is measurement. The effect sizes this experiment hunts for are plausibly a few points — and in the current agent-memory literature, judge noise exceeds effect sizes of that magnitude. An audit of LoCoMo, the benchmark most memory headlines route through, found [6.4% of the answer key wrong and the LLM judge accepting up to 63% of intentionally wrong answers](https://penfieldlabs.substack.com/p/we-audited-locomo-64-of-the-answer). Published numbers for competing systems on that benchmark span roughly 58–85%, moving by fifteen-plus points [depending on who runs the harness](https://github.com/getzep/zep-papers/issues/5). An experiment scored with that class of instrument would inherit the field's benchmark crisis and settle nothing.

So the program needs audited answer keys, non-judge metrics wherever tasks permit them (executable checks, exact-match facts, scored abstention), multiple replicates with reported variance, and judges validated against known-wrong answers before they score anything real. This is [evaluation as measurement](/thoughts/evaluation-resolution/), and here it is not methodological hygiene — it is the difference between an experiment and an anecdote. The resolution of the instrument bounds the size of the effect you are entitled to claim.

None of this is exotic. It is a preregistered factorial study with controls, run on benchmarks that already exist, at a cost within a single lab's discretionary budget. Somewhere between eight folders of Markdown and a verifier there may be the first evidence that artificial minds can be *grown*, not just trained — or the null result that ends this series' speculation honestly. Either outcome is worth having.

**What would change my mind:** if the compute-matched controls — especially Self-MoA and full-context — match every architectured instance and every heterogeneous society across long horizons, then persistent architecture is decoration and divergence is noise; and if between-architecture variance never separates from within-architecture replicate variance under audited, non-judge scoring, I would conclude that the mind is in the weights after all, and say so in this space.
