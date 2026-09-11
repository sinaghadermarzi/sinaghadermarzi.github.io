---
title: "Same Model, Different Minds"
# draft: true
date: 2026-08-11
description: "A falsifiable hypothesis: two identical checkpoints, given a year and different persistence policies, become measurably different minds."
---

Take two copies of the same frozen checkpoint. Same weights, same decoding parameters, same task stream, same compute budget. One difference only: instance A persists what it learns as prose — syntheses, arguments, notes that link to notes. Instance B persists what it learns as executable tools — scripts it wrote, tested, and kept. Run them for a year. Then hand both the same novel problem and watch.

My bet is that you are no longer watching the same mind twice. Not because anything changed inside the network — nothing did — but because each instance now thinks *through* a different accumulated structure, and the structure has become part of the thinker.

Stated as a hypothesis rather than a slogan: **identical base models under different persistence policies diverge behaviorally, and the divergence grows with horizon.** That is falsifiable, which is the point of this post.

## An experiment nobody has run

I should be clear about what is old here, because most of it is. Philosophy got to the conclusion decades ago: [Clark and Chalmers](https://doi.org/10.1093/analys/58.1.7) argued in 1998 that Otto's notebook is functionally part of Otto's memory — and a Markdown knowledge base is Otto's notebook with a faster reader. [Hutchins](https://pages.ucsd.edu/~ehutchins/documents/CockpitSpeeds.pdf) showed that the cognitive properties of a pilots-plus-instruments system "can differ radically from the cognitive properties of the individuals who inhabit them." If the artifact is part of the mind, then different artifacts make different minds. I am not inventing that inference; I am borrowing it.

What I could not find — and I looked, as of August 2026 — is anyone running the machine version as a controlled experiment: hold the weights and compute fixed, vary *only* the persistent external architecture, run long, and measure whether the instances become different. The absence is the increment I am claiming.

The hypothesis has a clean shape. Write a mind at time $t$ as the frozen weights plus its accumulated store:

$$
\mathcal{M}_i(t) = (\theta, A_i(t)), \qquad D(t) = d\!\left(\mathcal{M}_1(t), \mathcal{M}_2(t)\right)
$$

In words: each instance is the same weights $\theta$ paired with a different artifact store $A_i$ that grows over time, and $D(t)$ is the behavioral distance between them on held-out probes. The hypothesis predicts $D(t)$ grows with horizon and comes to exceed the run-to-run noise between two instances of the *same* architecture. If $D(t)$ plateaus into that noise, the hypothesis is dead. I sketch the full protocol — architectures, controls, endpoints — in [the companion post](/thoughts/growing-minds-in-the-lab/).

## The fragments we do have

No single result establishes this, but several establish pieces of the mechanism.

That accumulated artifacts change what an agent can do is the best-supported piece. [Voyager](https://arxiv.org/abs/2305.16291) grew a library of executable skills in Minecraft — compositional code that compounded capability (3.3× more unique items, key milestones up to 15.3× faster) and transferred to a new world. [ExpeL](https://arxiv.org/abs/2308.10144) did the declarative counterpart: insights distilled from experience and reused without touching a single weight. [Agent Workflow Memory](https://arxiv.org/abs/2409.07429) showed that workflows induced from past trajectories change downstream behavior on web tasks. And [Generative Agents](https://arxiv.org/abs/2304.03442) showed early on that memory-policy choices — what gets stored, reflected on, retrieved — shape behavior enough to matter. Different artifact classes, same lesson: the store is load-bearing.

A newer fragment, which I flag as a first observation from a weeks-old paper rather than an established result: a July 2026 [study of filesystem-based agent memory](https://arxiv.org/abs/2607.26637) found that changing the tool set alone reshapes the resulting memory store about as strongly as swapping the model. One paper, one setting — but it is the write-side mirror of my claim: the architecture around the model leaves a signature as deep as the model's own.

The closest cousin is [Multiagent Finetuning](https://arxiv.org/abs/2501.05707): copies of the same base model, specialized into generators and critics through interaction, sustaining self-improvement longer than a single self-improving model. Same starting weights, real divergence, measurable benefit — except the divergence lives in the *weights*, because each copy is finetuned on different data. My hypothesis is the stricter variant: freeze the weights entirely and ask whether the artifact layer alone can carry the differentiation. That the weight version works makes the artifact version plausible. It does not make it true.

## The persona null cuts both ways

Here is the result I keep turning over. [Zheng et al.](https://arxiv.org/abs/2311.10054) tested 162 personas across four model families and 2,410 factual questions and found that telling a model it is an expert does not make it one — persona prompts produced no gains, and often slight losses.

One reading favors me: cheap differentiation fails. If a label in the system prompt cannot make two instances into different minds, then whatever differentiates minds must be earned — accumulated, structural, external. That is exactly where my hypothesis places it.

But the other reading is just as available, and I want it on the record: maybe the base model's prior is simply a strong attractor. The same weights process whatever appears in context in the same characteristic style, and a year of different files might wash out the way a persona string does — a costume over an unchanged actor. The persona null proves prompts are too shallow to differentiate a mind. It does not prove artifacts are deep enough. Both readings survive the evidence we have, which is precisely why this needs to be an experiment and not an essay.

## Divergence may be disease

Two concessions, both serious.

First, nobody actually runs a frozen checkpoint for a year. In practice the underlying model gets swapped several times annually, and each swap may move behavior more than any accumulated store does. If model-generation churn dominates the artifact layer, my hypothesis becomes true but irrelevant — a second-order effect drowned by the first-order one. Whether artifact effects are non-negligible relative to churn is itself empirical, and I concede that current deployment practice implicitly bets against me.

Second, divergence is not improvement. Bad writes [propagate and compound](https://arxiv.org/abs/2505.16067) over an agent's deployment lifetime; [fewer than 0.1% poisoned memory records](https://arxiv.org/abs/2407.12784) can hijack behavior end-to-end; and in [Vending-Bench](https://arxiv.org/abs/2502.15840)'s twenty-million-token runs, even the best models had catastrophic episodes — forgotten orders, unproductive loops — for reasons unrelated to context limits. One recent preprint (single-author, fully synthetic data, unreplicated — weigh accordingly) even found that [memory-architecture rankings inverted as the store aged](https://arxiv.org/abs/2607.21962). So two diverged minds may just be two differently broken minds: distinct stale beliefs, distinct self-reinforcing errors, distinct blind spots. If differentiated minds are worth growing at all, they will need immune systems — provenance, verification, refresh — which is an argument I develop in [the ecology post](/thoughts/epistemic-ecology/). What the two phenotypes in my opening thought experiment would actually look like, essays versus tools, is the subject of [its own post](/thoughts/markdown-minds-and-tool-minds/).

Still: every fragment above points the same direction, and no result yet points the other way. The hypothesis is live, cheap to state, and — for the first time — affordable to test.

**What would change my mind:** a run of the experiment in which behavioral divergence between architectures plateaus into run-to-run noise — two instances with a year of different artifacts remaining as interchangeable as two random seeds. Nearly as fatal: a demonstration that swapping the checkpoint moves behavior an order of magnitude more than swapping the store, which would make the artifact layer real but decorative.
