---
title: "Creativity as Traversal"
draft: true
date: 2026-08-23
description: "Distant insights may be sequences of locally plausible hops, which is why link-following agents could be creative in a way embedding search alone is not."
---

This series began with a plumbing question — should an agent compile what it reads into a persistent wiki? — and has somehow arrived at cognitive typology and artificial minds. It is worth pausing on *how* the argument traveled, because the route is the point. Reconstructed honestly, my own chain of thought looked like this:

```text
LLM wiki --(works like)--> CPU cache
         --(generalizes to)--> a whole storage hierarchy
         --(brains have one too)--> multiple human memory systems
         --(which differ across people)--> cognitive types
         --(so, perhaps)--> artificial cognitive types
```

Every arrow is short and almost boring. Any engineer would grant each hop individually. But the endpoints are nowhere near each other: "cognitive typology" does not appear in the nearest-neighbor list of "markdown files," and no single similarity lookup makes that jump. A walk made it easily, because each step only had to be *locally* plausible.

I think this is a candidate mechanism for one kind of creativity — and a concrete reason link-following agents might be creative in a way flat retrieval cannot. But I want to claim it carefully. Psychology got here sixty years before agent memory did, the human evidence is correlational, and traversal at best generates insight *candidates*. Lineage first, then the computational claim, then the limits, then the experiment that would test it.

## Psychology measured this first

[Mednick defined the creative process in 1962](https://doi.org/10.1037/h0048850) as "forming associative elements into new combinations which either meet specified requirements or are in some way useful," and built the Remote Associates Test on a structural distinction: less creative people have *steep* associative hierarchies — a few strong, stereotyped associates that tail off fast — while creative people have *flat* ones, with many weaker, more remote associates available. That is a graph claim wearing 1962 clothing: creativity as a property of edge-weight distributions.

Network science eventually made it measurable. [Kenett, Anaki, and Faust](https://doi.org/10.3389/fnhum.2014.00407) mapped the semantic networks of low- and high-creative individuals and found the high-creative networks measurably more interconnected and less modular — flatter, in Mednick's sense. Steyvers and Tenenbaum had already shown (in *Cognitive Science*, 2005) that human semantic memory is a small-world graph: sparse, heavily clustered, with average path lengths around three hops — a topology in which a few shortcuts collapse distances dramatically. Schilling's small-world model of insight (*Creativity Research Journal*, 2005) turns that into a theory of the "Aha!": an insight is what it feels like when an atypical connection forms a shortcut and average path length abruptly drops — in her illustrative network, a single new link cuts it from about 2.74 to 1.90. And [Gray and colleagues' "forward flow"](https://doi.org/10.1037/amp0000391) measure found that how far people travel through semantic space during free association predicts creativity across domains. The title of this post is nearly their operationalization.

## Making remote association computable

[Bush saw the mechanism in 1945](https://www.theatlantic.com/magazine/archive/1945/07/as-we-may-think/303881/): the mind "operates by association — with one item in its grasp, it snaps instantly to the next that is suggested by the association of thoughts." What agent memory adds is a clean computational contrast between two retrieval questions:

$$
P(D \mid Q) \quad \text{versus} \quad P(D_{t+1} \mid D_t, Q, \text{path})
$$

In words: flat retrieval asks *which document is relevant to the query*; traversal asks *where should I go next, given where I already am and how I got here*. The first is one lookup; the second is a walk, and walks compound. Two hops of "obviously related" reach material that no single query would surface.

This is not just an aesthetic preference. DeepMind's [LIMIT paper](https://arxiv.org/abs/2508.21038) proves that for any embedding dimension there exist combinations of relevant documents that no single query vector can retrieve — flat similarity search has a mathematical ceiling, so *some* structural, multi-step mechanism is load-bearing. And [HippoRAG](https://arxiv.org/abs/2405.14831) shows one working instance: a knowledge graph plus Personalized PageRank — spreading activation, formalized — improving multi-hop question answering by up to 20% over flat baselines. A system whose corpus carries explicit links gets the walk almost for free, because each stored link is a [memoized past inference](/thoughts/links-as-stored-inference/); traversal composes those stored inferences into paths that nobody ever stored whole.

## Where I have to stop overselling

Four limits, stated plainly.

**The human evidence is correlational.** Kenett's flat networks travel with creativity; nothing shows they cause it. Flat structure could be a consequence of creative activity, or both could ride on something else. Kenett and Austerweil later [modeled retrieval in these networks as random-walk search](https://cogsci.mindmodeling.org/2016/papers/0066/index.html) — which cuts both ways: if an unguided walk over the right network suffices, the creativity lives in the network, not in any clever navigator.

**Traversal cannot reach everything.** Boden's taxonomy (in *The Creative Mind*) distinguishes combinational, exploratory, and transformational creativity. Link-following maps well onto the first, partly onto the second, and by construction not at all onto the third: transforming the space — changing what kinds of nodes and edges can exist — is precisely what walking the existing edges cannot do.

**Distance is a crude proxy.** Semantic remoteness is not value; Organisciak and colleagues showed in 2023 that LLM judges beat raw semantic distance at scoring divergent thinking. An agent optimized for forward flow alone would be a non-sequitur generator.

**Traversal generates; something else must verify.** Most remote associations are junk. Schilling's own model requires the shortcut to be *useful*, which means a critic is quietly doing half the work — the explorer/critic coupling I formalize as [artifact compatibility](/thoughts/artifact-compatibility/). And her "paradox of expertise" — densely trained clusters both enable and block insight — predicts a failure mode for link-heavy agents: hub dominance, where every walk collapses into the same well-worn neighborhoods.

## The A/B test I want run

The claim reduces to an experiment someone could run in a month. Take one corpus with an explicit link graph — a mature personal vault, or a wiki an agent compiled itself. Same base model, same prompts, same total token budget, two retrieval conditions: **A** retrieves by embedding similarity only (spending its budget on more top-k reads); **B** starts at a seed note and walks links under a bounded hop budget, reading as it goes. The task: propose new connections, hypotheses, or project ideas linking the seed topic to anything in the corpus. Score the outputs blind, novelty and usefulness separately, with human raters primary and LLM judges only as a cross-check. Then measure, in the corpus graph, the distance between each idea's seed and the material it actually drew on.

The traversal thesis predicts B reaches farther at equal usefulness. The deflationary result is just as informative: if A matches B's novelty-at-usefulness, the graph machinery is decorative and this post's computational half is wrong. Either way you learn something — which is why this is one cell of the larger factorial program in [growing minds in the lab](/thoughts/growing-minds-in-the-lab/).

Traversal is not a theory of creativity. It is one computational ingredient — the generator half of a generator–critic pair — with sixty years of suggestive human evidence and one clean way to be falsified.

**What would change my mind:** A well-run version of the A/B experiment in which embedding-only agents match link-following agents on blind novelty-at-equal-usefulness would gut the computational claim; causal interventions showing that flattening or densifying an associative network leaves creative output unchanged would gut the psychological one. And a system routinely producing Boden-style transformational leaps with no traversal machinery at all would demote traversal from ingredient to bystander.
