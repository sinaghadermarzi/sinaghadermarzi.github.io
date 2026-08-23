---
title: "A Beautiful Hierarchy Is Not Intelligence"
draft: true
date: 2026-08-23
description: "Organizing an agent's memory pays off in better answers on some workloads and only cheaper answers on others — and no one reports the full ledger."
---

I have reorganized my notes directory at least five times in the past decade. Flat files, then folders by project, then folders by area, then tags, then back to folders. Each reorganization felt like progress, and each produced roughly the same number of good ideas as the system before it — the reorganizing was the hobby; the thinking happened elsewhere. Humans love tidy trees. It turns out LLM agents do too: give one a memory directory and file tools, and it will cheerfully refactor its own knowledge into a clean taxonomy. Watching it do so *feels* like watching it get smarter.

But a beautiful hierarchy is not intelligence. Organization is an investment, and an investment has to pay out in one of exactly two currencies: better answers, or cheaper answers. The aesthetic pleasure of a well-nested tree does not distinguish between them, and most talk about agent memory doesn't either. The causal chain that would justify the effort is specific — organization improves routing and retrieval, which improves reasoning or lowers cost — and every link in that chain is an empirical claim, not a design preference.

## The first measurement is deflationary

I would love to present that as my own unasked question, but the field got to it weeks before I wrote this down. In July 2026, a group spanning UIUC, UCSD, and Adobe published [the first systematic study of filesystem-based agent memory](https://arxiv.org/abs/2607.26637) — the deployed default, a directory tree of Markdown files that the agent itself reads, writes, and reorganizes through generic file tools. They separate the roles cleanly (a management agent organizes the store, a search agent answers from it, an execution agent consumes it) and measure what the organizing actually does.

Three findings, all deflationary in an instructive way. First, organization buys economy: "organized stores roughly halve retrieval cost where material is large." Second, it buys nothing else: "no agent we measure converts organization itself into better answers." Third, it does not even preserve itself: organization erodes over time for all but the strongest management agents, and a freely reorganizing pass tends to silently condense content away unless an explicit preservation rule is added. The tidy tree is a cost optimization with a maintenance bill, decaying by default.

The necessary hedge: this is one paper, one setting, unreplicated, and only weeks old. (The same study found that "changing the tool set alone reshapes the store as strongly as swapping the model," which is a different post's thesis entirely — see [Memory Is an Action Space](/thoughts/memory-action-space/).) But a first observation pointing this way should unsettle anyone whose intuition says structure is self-evidently good.

## Where organization does buy correctness

Because there is opposite evidence, and it is older and better established. [RAPTOR](https://arxiv.org/abs/2401.18059) builds a tree of recursive summaries over a corpus and retrieves from multiple levels of abstraction; with GPT-4 as the reader it reached 82.6% on QuALITY against a prior best of 62.3%. (Honest caveat: that prior best was a DeBERTa-class system, so much of the 20-point gap is reader strength — RAPTOR's own like-for-like comparisons against flat retrieval with the same reader show gains of a few points. Real, not twenty.) [HippoRAG](https://arxiv.org/abs/2405.14831) precomputes a knowledge graph and runs Personalized PageRank over it, beating state-of-the-art RAG by up to 20% on multi-hop questions while matching expensive iterative retrieval at 10–20× lower cost. And [a systematic comparison of memory representations](https://arxiv.org/abs/2412.15266) — chunks versus triples versus atomic facts versus summaries — found that different structures win on different tasks, with no universal best.

So organization halves cost without improving answers, and organization improves answers by double digits. Both results are real. The reconciliation is that they measure different workloads:

| Workload | What organization buys | Evidence |
|---|---|---|
| Recall a fact you stored | cheaper search, same answers | [filesystem study](https://arxiv.org/abs/2607.26637) |
| Integrate a theme across a corpus | better answers | [RAPTOR](https://arxiv.org/abs/2401.18059) |
| Multi-hop association | better answers, cheaper too | [HippoRAG](https://arxiv.org/abs/2405.14831) |
| A store that grows for months | erosion, unless actively maintained | [filesystem study](https://arxiv.org/abs/2607.26637) |

The pattern I read in that table: organization converts to correctness only when the question demands cognition that flat retrieval cannot perform at read time. A summary tree has already done the integrating; a graph has already done the associating. When the question is a lookup, all that precomputed work is dead weight — pleasant to browse, useful only as a cheaper index. Organization is precomputation, and precomputation pays exactly when the workload will ask for what you precomputed. That is the thesis of [the missing science of semantic memoization](/thoughts/science-of-semantic-memoization/), restated at the level of structure.

## The ledger nobody publishes

This reframing has an uncomfortable consequence for how the field reports results. If organization's payoff is sometimes quality and sometimes economy, a single accuracy number cannot evaluate a memory system — yet accuracy is mostly what gets reported, on benchmarks shaky enough that [an audit found 6.4% of LoCoMo's answer key is simply wrong](https://penfieldlabs.substack.com/p/we-audited-locomo-64-of-the-answer). The economy column, meanwhile, is reported only when it flatters. In [Mem0's own evaluation](https://arxiv.org/abs/2504.19413), the full-context baseline beats Mem0 on judged accuracy — roughly 73% versus 68%, a comparison [a competitor was delighted to point out](https://blog.getzep.com/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory/) — while Mem0 delivers a 91% cut in p95 latency and about 90% fewer tokens per query. Read plainly, and with the caveat that these are vendor-run numbers on a contaminated benchmark: today's memory products mostly sell economy, not correctness. That is a legitimate product. It is just not the product on the label.

And two whole columns of the ledger are almost never printed at all. Write cost is invisible — secondary analyses of [MemoryAgentBench](https://arxiv.org/abs/2507.05257) report Mem0's memory-construction time at roughly 20,000× BM25's, a figure I have not seen in any headline. Maintenance cost is invisible too, though the erosion finding says it is the term that decides whether the whole scheme survives its second month. So here is the report card I want every memory paper to fill in:

$$
U \;=\; f\big(\Delta Q,\; C_{\text{read}},\; C_{\text{write}},\; C_{\text{maint}},\; R(t)\big)
$$

In words: the utility of a memory design is a function of the answer-quality change it causes, the cost of reading from it, the cost of writing to it, the cost of keeping it organized, and how well all four hold up as the store ages. Nothing here is novel as economics; what is genuinely open is the reporting convention — as far as I can find, no published system reports all five terms, and most report one, chosen favorably. An evaluation blind to four-fifths of the invoice cannot rank architectures, no matter how precise its accuracy column looks — the same instrument-resolution failure I argued about in [evaluation as measurement](/thoughts/evaluation-resolution/).

Until that ledger is standard practice, my working rule is deliberately unsentimental. Ask what the workload needs before admiring the structure. If it needs integration or association, organization is cognition done early, and worth real money. If it needs lookup, organization is a discount coupon that expires unless somebody pays to maintain it. And if you find yourself refactoring the taxonomy for the fifth time — agent or human — check which currency you are actually earning.

**What would change my mind:** A replication of the filesystem result in which stronger management agents *do* convert self-maintained organization into better answers on plain-lookup workloads would collapse my workload-dependence claim into a temporary skill gap. So would a single architecture that dominates flat baselines on both quality and full-ledger cost across lookup, integrative, and multi-hop workloads alike — if a universal winner exists, "it depends on the workload" was just a polite description of 2026's immaturity.
