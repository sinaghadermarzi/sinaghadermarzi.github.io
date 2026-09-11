---
title: "Persistence Is Not Presence"
# draft: true
date: 2026-08-11
description: "Writing knowledge down does not make retrieval disappear; a useful hit in a persistent store is a conjunction of four fragile successes."
---

In April, [Andrej Karpathy named](https://x.com/karpathy/status/2039805659525644595) a pattern half the field had been circling for years: instead of RAG — where, as [his gist puts it](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), "the LLM is rediscovering knowledge from scratch on every question. There's no accumulation" — have the model incrementally "compile" a wiki, so that "the wiki is a persistent, compounding artifact. The cross-references are already there. The contradictions have already been flagged."

I find the idea genuinely compelling, and I use a version of it daily. But it ships bundled with a fantasy, and I keep meeting the fantasy in the wild: *once the understanding is compiled, the agent will never have to search again.* Retrieval was the tax we paid for not writing things down; the wiki abolishes the tax.

The fantasy confuses persistence with presence. What is *present* — in the context window, under the model's attention right now — gets used. What is *persistent* merely exists, somewhere on disk. That gap is not a new discovery of mine; it is the exact gap [MemGPT](https://arxiv.org/abs/2310.08560) engineered around in October 2023 with its operating-system analogy: the context window is RAM, everything else is paged out, and something has to do the paging. Writing knowledge down does not eliminate retrieval. It changes what retrieval means — from "re-explore the world from scratch" to "recover the right previously compiled understanding" — and that recovery is harder than it looks, because it is a conjunction.

## A useful hit is a conjunction

For a persistent store to actually save work on a given question, four things must all be true at once:

$$
P(\text{useful hit}) \;=\; P(\text{compiled}) \times P(\text{found} \mid \text{compiled}) \times P(\text{sufficient} \mid \text{found}) \times P(\text{trusted} \mid \text{sufficient})
$$

In words: the relevant understanding must have been written down in the past; the system must locate that page now; the page must contain what *this* question needs; and what it contains must still be true. Multiply four probabilities each meaningfully below one, and "give it a wiki" stops sounding like the end of retrieval.

What makes this worth writing down is that each factor fails differently.

**Compiled** fails at write time, invisibly: the question you have today is one nobody thought to synthesize six months ago. Admission — deciding what deserves compilation at all — is its own problem, which I take up in [The Write Path](/thoughts/memory-write-path/).

**Found** fails at query time, and this is the failure people underestimate most, because a wiki's page names and vocabulary are its address space. If today's question doesn't share vocabulary with yesterday's synthesis, lookup becomes semantic inference, and the evidence below says models are startlingly bad at that.

**Sufficient** fails through compression. Compilation is lossy by design — that's the point — but the detail summarized away is sometimes the detail the new question needs. There is even a first measurement of how bad the default is: a [July 2026 study of filesystem-based agent memory](https://arxiv.org/abs/2607.26637) found that when agents reorganize their own Markdown stores, the reorganizing pass "silently condenses content unless one preservation rule is added." (One preprint, weeks old, unreplicated — a first observation, not an established phenomenon — but it matches my own experience uncomfortably well.)

**Trusted** fails with time and with bad writes. A hallucination committed during compilation becomes a persistent, retrievable, confidently cited error — a cache that corrupts what it stores. And even honest syntheses go quietly stale as the world moves, which is the subject of [Knowledge That Stays Alive](/thoughts/living-knowledge-bases/).

## Finding is harder than it looks — even with the answer in hand

The strongest evidence that persistence doesn't dissolve retrieval comes from an unfair-sounding experiment: skip the store entirely, place the knowledge directly *into the context window*, and watch retrieval fail anyway.

[NoLiMa](https://arxiv.org/abs/2502.05167) is the sharpest version. Standard needle-in-a-haystack tests let models cheat with literal keyword matching; NoLiMa constructs needles with minimal lexical overlap, so the model must infer the latent association — exactly what a wiki lookup requires when the question is phrased differently from the page. The result: at a mere 32K tokens, 10 of 12 long-context models drop below 50% of their own short-context baselines. GPT-4o falls from 99.3% to 69.7%. [Lost in the Middle](https://arxiv.org/abs/2307.03172) found something stranger: with the answer sitting among 20–30 documents in context, GPT-3.5-Turbo could score *below its own closed-book performance* of 56.1% — being handed the answer buried in material was worse than being handed nothing. And [Chroma's context-rot report](https://research.trychroma.com/context-rot) shows the degradation is general across 18 frontier models, well before advertised context limits.

Cross sessions and the picture holds. [LongMemEval](https://arxiv.org/abs/2410.10813) measures a 30% accuracy drop for commercial assistants and long-context models on remembering information across sustained interaction — and, usefully, decomposes the problem into indexing, retrieval, and reading stages, each of which can independently fail. That decomposition is the layered route made explicit: task → working context → session memory → persistent wiki → raw sources → world. Persistence adds layers to route through; it doesn't remove the routing.

So even presence is not quite presence. A page you compiled, found, and loaded still has to win the model's attention against everything else in the window. The wiki's promise has to survive all four conjuncts *plus* this last one.

## The honest scoreboard

I should not overstate the case in the other direction, because the current evidence cuts against memory systems too — just differently. In Mem0's own published evaluation, the full-context baseline beats the memory system on accuracy (roughly 73% vs 68% on the LLM-judge metric, [as Zep's team pointed out while reading Mem0's own table](https://blog.getzep.com/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory/)); what [Mem0 wins decisively](https://arxiv.org/abs/2504.19413) is economics — p95 latency down from 17.12s to 1.44s, roughly 90% fewer tokens. Today's memory products mostly buy cost and latency, not correctness. Meanwhile the brute-force alternative keeps improving: context windows keep growing, and [prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) prices cache reads at 10% of base input cost — re-reading everything is 90% off. Whether compiled persistence beats cached brute force is an empirical crossover point, not a settled principle; context rot and unboundedly growing histories cap the brute-force strategy, but nobody has measured where the lines cross.

Here is what I think persistence actually buys, stated without the fantasy: it converts one monolithic problem — "figure this out again" — into four smaller, *named* problems: admission, findability, sufficiency, and trust. Named problems can be instrumented, measured, and engineered separately; MemGPT's paging, LongMemEval's stage decomposition, and the filesystem paper's preservation rule are each attacks on exactly one conjunct. That conversion is real progress. But a conjunction of four engineered subsystems is a very different promise from "it will never search again" — and mistaking the second for the first is how you end up with a beautifully compiled wiki that quietly answers the wrong question. What the store is, once you stop calling it a cache, is the subject of [More Than a Cache](/thoughts/semantic-storage-hierarchy/).

**What would change my mind:** a longitudinal, matched-budget study showing a compiled knowledge store whose end-to-end useful-hit rate approaches its retrieval hit rate — that is, where the compiled, sufficient, and trusted factors hold so reliably that finding is the only failure mode left; or evidence that growing context windows plus cheap cached re-reading beat compiled stores on correctness *and* cost at personal-corpus scale, which would make the conjunction moot because presence would be affordable for everything.
