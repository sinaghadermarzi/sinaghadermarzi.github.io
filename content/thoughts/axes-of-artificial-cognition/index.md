---
title: "The Axes of Artificial Cognition"
draft: true
date: 2026-08-23
description: "Agent memory architectures are better described as coordinates on a few defended axes than by the names of the technologies they are built from."
---

Ask an engineering team what their agent's memory architecture is and you will get the name of a technology: "a vector store," "a knowledge graph," "Markdown plus grep," "RAG with a scratchpad." I have given every one of these answers myself. They are genuinely informative — about infrastructure. About cognition they say less than we assume. In [Mem0's own evaluation](https://arxiv.org/abs/2504.19413), bolting a knowledge graph onto the base system — the change that moves it into a different technology category entirely — shifted overall benchmark performance by about two percent, helping some question types while hurting others. Meanwhile the first systematic study of filesystem memory reported that [changing the tool set alone reshapes the memory store as strongly as swapping the underlying model](https://arxiv.org/abs/2607.26637) — one July 2026 result, unreplicated, but pointing at something I already believed: the same technology can host very different minds, and different technologies can host nearly the same mind. Technology names carve the design space at the wrong joints. What we want instead is what every maturing engineering field eventually wants: coordinates.

## The axes we inherited

Honesty first, because most of the good axes are old, and I want to be precise about which ones I am borrowing.

Declarative versus procedural — does the system persist descriptions or capabilities — belongs to cognitive science, not to me. [Cohen and Squire showed in 1980](https://www.science.org/doi/10.1126/science.7414331) that amnesic patients acquire a mirror-reading skill at a normal rate and retain it for months while being unable to remember the training sessions themselves: knowing-how survives when knowing-that is gone. [ACT-R](https://books.google.com/books/about/How_Can_the_Human_Mind_Occur_in_the_Phys.html?id=eYZXEtfplyAC) has engineered artificial minds around exactly this split — declarative chunks, procedural production rules — since the 1980s. An agent with a Markdown store and a script library is ACT-R's internals externalized onto a filesystem; I walked through the two resulting phenotypes in [Markdown Minds and Tool Minds](/thoughts/markdown-minds-and-tool-minds/). Episodic versus semantic — records of particular experiences versus timeless synthesized knowledge — is Tulving's, from his 1972 chapter in *Organization of Memory*. Even the hedge is inherited: [Squire himself later argued](https://www.sciencedirect.com/science/article/abs/pii/S1074742704000735) that the field had to move "beyond dichotomies" to a picture of many interacting systems.

The agent-memory field has its own inherited scheme too. The standard survey ([ACM TOIS 2025](https://arxiv.org/abs/2404.13501)) organizes the design space by memory *sources* (in-trial, cross-trial, external), *forms* (textual versus parametric), and *operations* (writing, management, reading). Those are real, useful dimensions. What interests me is what they leave out.

## Two axes the taxonomies don't have

**Node versus link.** Where does the knowledge live: inside rich, self-contained documents, or in the topology of relations among small ones? The polarity itself is old in other fields — Bush's [1945 argument that selection by association should replace indexing](https://www.theatlantic.com/magazine/archive/1945/07/as-we-may-think/303881/), Luhmann's insistence that a note ["receives its quality only from the network of links and back-links"](https://luhmann.surge.sh/communicating-with-slip-boxes) — but as far as I can find, it appears nowhere as an explicit axis in the agent-memory taxonomies. And it behaves the way a real axis should: both poles win somewhere. [HippoRAG's](https://arxiv.org/abs/2405.14831) graph-plus-Personalized-PageRank memory beats strong flat retrieval by up to 20% on multi-hop questions, while Microsoft's own [LazyGraphRAG](https://www.microsoft.com/en-us/research/blog/lazygraphrag-setting-a-new-standard-for-quality-and-cost/) showed that deferring graph construction to query time matches full GraphRAG at 0.1% of the indexing cost. Eager links pay on some workloads and are pure overhead on others — a tradeoff, not a ladder. [Node Minds and Link Minds](/thoughts/node-minds-and-link-minds/) takes this axis apart on its own.

**Archival versus living.** How does the store relate to time: is it a preserved record of past cognition, or does it maintain an active relationship with a changing world — refresh cadences, invalidation, watching for the new fact that quietly breaks an old conclusion? Nearly every deployed memory system sits at the archival pole. The living pole has early existence proofs, such as [DYNA's continuously updated temporal-graph memory](https://arxiv.org/abs/2606.15778) (June 2026; no relation to Sutton's classic Dyna). The most striking evidence that the time dimension is load-bearing comes from [a July 2026 preprint](https://arxiv.org/abs/2607.21962) that evaluated memory architectures at two horizons and found the rankings *invert* as memory ages: the compact curated memory that led at three weeks fell from 96% to 72% recall by nine weeks, while a provenance-typed graph rose to about 90%. I want that result to be true, so I have to flag it hard: it is a single unreplicated preprint on fully synthetic histories. But notice that sources, forms, and operations have no slot for the question it asks. I made the case for the living pole in [Knowledge That Stays Alive](/thoughts/living-knowledge-bases/).

## Coordinates, and the discipline of not adding more

Put together:

$$
\mathcal{A} \;=\; \big(x_{\text{decl}\leftrightarrow\text{proc}},\;\; x_{\text{node}\leftrightarrow\text{link}},\;\; x_{\text{archival}\leftrightarrow\text{living}}\big)
$$

In words: describe a memory architecture by its position on each axis — how much of its persisted cognition is executable procedure rather than description, how much of its knowledge resides between objects rather than inside them, and how actively it maintains its contents against the world — rather than by the technology in its name.

The immediate payoff is disambiguation. Names collide; coordinates separate:

| The name on the box | decl↔proc | node↔link | archival↔living |
|---|---|---|---|
| "Vector store over notes" | declarative | node | archival |
| Zettelkasten-style memory ([A-MEM](https://arxiv.org/abs/2502.12110)) | declarative | link | archival |
| Skill library ([Voyager](https://arxiv.org/abs/2305.16291)) | procedural | node, drifting linkward as skills compose | archival |
| Living topic file with a refresh policy | declarative | node | living |

Two systems that share a row are similar minds whatever they are built on; two that share only a technology name may have nothing cognitive in common. And coordinates make the right empirical question askable — which positions are Pareto-optimal for which workloads — a question we already know has no single answer, since [chunks, triples, atomic facts, and summaries each win on different tasks](https://arxiv.org/abs/2412.15266).

Now the discipline. Having found two axes the taxonomies lack, the temptation is to keep going: explorer versus verifier, simulator versus symbolic reasoner, provenance-heavy versus compression-heavy, local specialist versus global synthesizer. I feel the pull and I am resisting it, because an axis should earn its place: it must be variable independently of the others, it must have working existence proofs at both poles, and moving along it must change measurable behavior. My three pass that test — barely, in the archival–living case, on weeks-old evidence. The candidates above currently pass none of it; they are hypotheses about structure, and I hold them as exactly that. A design space with three defended dimensions is an instrument; one with eight speculative dimensions is a horoscope. Two of the defended axes cross into a two-by-two grid whose emptiest quadrant is, I think, the most interesting territory in the whole space — but that is the next post.

**What would change my mind:** A controlled comparison in which agents placed at different node–link and archival–living coordinates — same base model, matched compute and workload — showed no systematic behavioral differences would demote my two new axes to implementation detail and leave the inherited taxonomies sufficient. Independent replications failing to reproduce the ranking-inversion result on non-synthetic histories would remove the strongest current evidence for the archival–living axis. And if one coordinate turned out to Pareto-dominate across realistic workloads, the space would still exist, but only one point in it would matter.
