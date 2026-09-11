---
title: "Link Metabolisms"
draft: true
date: 2026-08-23
description: "Two knowledge graphs on identical storage can be different minds, because what distinguishes them is which kinds of links they emit — a knob that LLMs have just made cheap to turn."
---

Here are two knowledge graphs, each grown over a year of one agent's work, and alike in every inventory statistic — node counts, edge density, storage layer, even the base model behind them. Rendered side by side, they look interchangeable. Then read the edges. In the first graph, almost every link says *causes* or *is-a*. In the second, the dominant types are *conflicts-with* and *supported-by*. These are not two copies of one mind holding different content. They are different minds. The first has been compiling an explanatory model of its domain; the second has been maintaining a docket of live disputes with the receipts attached. Ask each of them "what should I believe about X?" and they will not merely return different answers — they will perform different cognitive operations to produce them.

I have argued that [a link is a stored inference](/thoughts/links-as-stored-inference/): an explicit edge memoizes a judgment some past act of cognition made. This post takes the next step. If links store inference, then the distribution of a mind's link *types* is a record of the inferences it habitually runs — its metabolism, in the plain sense of what it ingests and what typed structure it deposits. And the reason this is worth writing about now, rather than twenty years ago, is not that anyone lacked a vocabulary of link types. It is that the cost of writing them just collapsed.

## The taxonomy is fifty years old

Nothing about typed links is new, and I want to lead with that. Kunz and Rittel's IBIS, from 1970, structured planning arguments as issues, positions, and arguments joined by typed links — supports, objects-to, responds-to — precisely because untyped association was too weak to carry deliberation. Scholarly citation got its own type system in [CiTO](https://doi.org/10.1016/j.websem.2012.08.001), which is machine-readable and admirably blunt: `cito:supports` means "the citing entity provides intellectual or factual support for statements, ideas or conclusions presented in the cited entity," alongside `disputes`, `extends`, `refutes`, `qualifies`. Joel Chan's [discourse graphs](https://oasislab.pubpub.org/pub/54t0y9mk/release/3) decompose research literatures into Questions, Claims, and Evidence joined by supports/opposes/informs edges. NLP even isolated the learnable primitive a decade ago: [SNLI](https://arxiv.org/abs/1508.05326) framed entailment/contradiction/neutral as a classification problem over 570,000 sentence pairs — a *conflicts-with* detector as supervised learning.

Biology got there first, as usual. The stigmergy literature — coordination through persistent traces left in a shared environment — [distinguishes quantitative from qualitative traces](https://doi.org/10.1162/106454699568700): graded signal intensities like ant pheromone trails, versus discrete environmental configurations that trigger categorically different behaviors, like the stages of termite construction. Termite colonies have a type system for their external memory. As precedents go for "trace semantics is a real variable," that is about as strong as it gets.

## The bottleneck was always the writer

So if the vocabularies have existed for fifty years, why do almost none of our knowledge systems use them? Because every one of these systems died, or shrank into a niche, on the same rock: the cost of emission. gIBIS, the hypertext implementation of IBIS ([Conklin and Begeman, 1988](http://doi.acm.org/10.1145/45941.45943)), is remembered in its own literature as much for the friction of making contributors classify every utterance as for its considerable virtues. The Semantic Web asked the entire web to hand-author subject–predicate–object triples, and the web declined. Underneath the cost problem sits a reliability problem: trained annotators routinely disagree about whether a citation *disputes* a claim or merely *qualifies* it. Typed-link systems have historically failed on write cost and writer disagreement — not on read value.

That is the variable that moved. An LLM emits typed links as a side effect of processing. Contradiction detection is a learnable classification — imperfect, and demonstrably brittle under adversarial pressure, which is the entire point of [ANLI](https://arxiv.org/abs/1910.14599) — but brittle-and-nearly-free is a different engineering regime from reliable-and-expensive. Systems already do this quietly: [HippoRAG](https://arxiv.org/abs/2405.14831) has the model extract entity relations into a graph at indexing time and then retrieves by walking them; [Zep](https://arxiv.org/abs/2501.13956) maintains a temporal knowledge graph of agent memory as conversations stream past (a vendor evaluating its own product — calibrate accordingly). Emission went from a profession nobody would fund to a background process nobody notices.

## Emission policy is now a knob

Here is the claim I believe is unoccupied. Once emission is nearly free, every persistent mind has an **emission policy** — chosen, not inherited:

$$
\pi\left(r \mid a, b, C\right), \qquad r \in \mathcal{R} \cup \{\varnothing\}
$$

In words: a link metabolism is a policy $\pi$ that, given two artifacts $a$ and $b$ encountered in context $C$, decides which typed relation $r$ — where "none" is a legitimate answer — to write between them, over a chosen vocabulary $\mathcal{R}$. Hold the model, the storage, and the retrieval fixed, and $\pi$ alone can differentiate minds:

| Emission bias | What accumulates | Plausible strength | Plausible failure mode |
|---|---|---|---|
| Causal | an explanatory model | prediction, intervention | confident just-so stories |
| Analogical | cross-domain bridges | transfer, creative reach | superficial resemblance |
| Contradictory | a docket of disputes | critique, error detection | corrosive indecision |
| Hierarchical | a taxonomy | coverage, organization | brittle categories |
| Provenance | an audit trail | trust, graceful aging | bookkeeping overhead |

Every row of that table is a hypothesis, not a finding. But there is one early longitudinal datapoint suggesting that edge semantics interact with time. A [recent preprint on longitudinal memory evaluation](https://arxiv.org/abs/2607.21962) reports that a compact curated-map memory led at a three-week horizon (roughly 96% recall) and fell to roughly 72% by nine weeks, while a provenance-typed graph rose to about 90% — the ranking of architectures inverted as the memory aged. I attach every warning label it deserves: it is weeks old, apparently single-author, run on fully synthetic "life-script" data, with one implementation per architecture and no replication I know of. But as a first observation it points exactly where a metabolism view predicts: provenance edges are the ones that let old beliefs be re-verified rather than merely re-trusted.

What I cannot find anywhere is emission policy treated as the independent variable. Same corpus, same base model, same retrieval; instantiate a contradiction-heavy $\pi$ and a causal-heavy $\pi$; run both for months; measure what each mind becomes good at. That experiment used to cost an annotation team and a year. It now costs a system prompt and an evaluation harness. Emission policy is the [write path](/thoughts/memory-write-path/) applied to edges instead of nodes — and its most interesting readout is relational: does the contradiction-heavy mind's graph make a better input for a critic than for an explorer? That is one measurable column of the [artifact-compatibility matrix](/thoughts/artifact-compatibility/).

Three honest problems before anyone gets excited, me included. Cheap emission is not curation: a mistyped link is a stored error with all the durability of stored inference, and a graph that grows edges faster than it verifies them is memoizing its own mistakes. Second, annotator disagreement did not disappear — it moved into the model; if the same model at two temperatures types the same pair differently, $\pi$ is noisier than the word "policy" implies. Third, types may simply not matter downstream: a strong reader at query time might recover relation semantics from untyped adjacency, making $\pi$ decoration. Each of these is exactly the kind of question the newly cheap experiment can settle — which is, in the end, the whole argument.

**What would change my mind:** A controlled comparison in which minds with sharply different emission policies converge to indistinguishable downstream behavior — because query-time reading recovers relation semantics from untyped edges — would demote link types from metabolism to decoration. Evidence that LLM-emitted types drift too much across runs to hold $\pi$ fixed would be nearly as fatal, since a knob you cannot hold still is not an experimental variable. And a failed replication of the provenance-ages-well result would remove the one longitudinal datapoint this post leans on.
