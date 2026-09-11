---
title: "Evals Are the New Engineering Artifacts"
date: 2026-05-31
description: "How AI systems change the role of specifications and tests."
---

Traditional software relies on explicit specifications. A function has a contract, a test asserts the contract, and CI enforces it forever after. Nearly everything we call engineering discipline — code review, regression suites, refactoring with confidence — rests on a single assumption: correct behavior can be written down before the system exists and checked mechanically afterward.

AI systems increasingly require a different contract: the ability to measure whether behavior is good.

The old assumption breaks quietly, not loudly. A component built on a language model doesn't have an input–output contract; it has a behavior distribution. For any interesting task — summarize this note, extract these entities, decide whether this record matches that one — the space of acceptable outputs is large, fuzzy at the edges, and impossible to enumerate in advance. You cannot assert your way to "this summary is faithful" or "this extraction caught every name." The unit of quality stops being the assertion and becomes the measurement: a labeled sample, a rubric, a scoring function, and a number with error bars.

Evaluation becomes a core engineering artifact.

I mean *artifact* in the literal, unglamorous sense: a thing that is versioned, reviewed, diffed, and maintained, that other work depends on, and that outlives any particular model or prompt. When the system and the eval disagree, one of them has a bug — and it is a genuine investigation to find out which. Teams that treat evals as scaffolding to be thrown away after launch keep relearning the same lesson: the model is swappable, the prompt is swappable, the *definition of good* is the part you accumulate.

Three consequences follow once the eval is the load-bearing artifact.

**The eval is the spec.** Where behavior can't be fully specified, the evaluation set *is* the closest thing to a specification the system will ever have. Every judgment call — does a nickname count as PHI? is a paraphrase faithful? — lives in the labels and the rubric, not in a requirements document nobody can execute. Arguments about what the system should do become concrete proposals to change the eval, which is exactly where such arguments are cheapest to settle.

**Whatever is measurable becomes optimizable.** This is the compounding payoff. The moment "good" is a number, every upstream text — prompts, few-shot examples, annotation guidelines — turns from prose into parameters. [DSPy](https://dspy.ai/) makes this explicit by treating an LLM pipeline as a program compiled against a metric, and optimizers like [GEPA](https://arxiv.org/abs/2507.19457) evolve the instructions themselves, using the model's own reflection on failures to propose revisions and the eval to keep or kill them. I use exactly this loop in my [current work](/work/) on PHI de-identification: the annotation guideline that steers the model is an optimizable artifact, refined against labeled evaluation sets — so a guideline change is an experiment with a measured result, not a debate between two plausible paragraphs.

**Evals create the feedback loop that lets intelligent systems improve.** A regression suite tells you a refactor didn't break the contract; an eval tells you a new model, a cheaper model, a reworded prompt, or a restructured pipeline didn't break the behavior — and by how much it helped. That's what makes iteration safe at all. It's also perfectly executable discipline today, not a manifesto: in my [agentic-lab](https://github.com/sinaghadermarzi/agentic-lab), [evaluation is a chapter of runnable machinery](https://github.com/sinaghadermarzi/agentic-lab/blob/main/04-evaluation.ipynb) — graders wired into the harness, so the chapters that follow can make claims with scores attached instead of adjectives.

The honest caveat is that an eval is an instrument, and instruments have finite resolution. A score is an estimate from a sample; a rubric encodes one set of judgment calls; anything optimized hard enough against a fixed metric will eventually exploit the metric instead of the task. That doesn't weaken the argument — nobody abandons thermometers because they have error bars — but it does mean the eval itself needs the scrutiny we used to reserve for code: provenance for labels, review for rubric changes, held-out sets for the optimizer. How much signal an evaluation can actually carry — its resolution — is a question that deserves its own essay.

The shift, compressed: in traditional software the durable artifacts were the code and its tests. In AI systems the model is rented, the prompt is disposable, and the pipeline gets rewritten twice a year. What accumulates — what defines success, enables feedback loops, and lets the system get better on purpose rather than by accident — is the evaluation. Build it like it's the asset, because it is.
