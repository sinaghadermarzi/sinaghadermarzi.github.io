---
title: "A Two-by-Two for Artificial Minds"
draft: true
date: 2026-08-23
description: "Crossing the declarative–procedural axis with the node–link axis yields four kinds of artificial minds, and the least explored quadrant is the most interesting one."
---

Here is the whole post in one table. Take the two axes this series has walked one at a time — what a long-lived agent persists: descriptions or operations ([Markdown minds and tool minds](/thoughts/markdown-minds-and-tool-minds/)); and where the persisted value lives: inside rich artifacts or in the topology between small ones ([node minds and link minds](/thoughts/node-minds-and-link-minds/)) — and cross them:

|  | **Node-major** (value in the objects) | **Link-major** (value in the topology) |
|---|---|---|
| **Knowledge-major** | Wiki pages, essays, summaries | Zettelkasten, conceptual graphs |
| **Tool-major** | A `scripts/` directory | Tool-composition graphs |

Three of these cells are crowded. The fourth is nearly empty, and the empty cell is the reason this post exists. What follows is not a classification of products — real systems are mixtures, and the grid deliberately holds at least one more axis fixed (archival versus living). It is a map: three settled provinces and one blank region.

## Borrowed axes, honest grid

Neither axis is mine. Declarative versus procedural is inherited from memory psychology — Cohen and Squire made it canonical, and ACT-R has built engineered minds around exactly that split, declarative chunks on one side and production rules on the other, since the 1980s. Node versus link is older still: [Vannevar Bush](https://www.theatlantic.com/magazine/archive/1945/07/as-we-may-think/303881/) argued in 1945 that the mind "operates by association" rather than by indexing, and Luhmann's slip box ran on the principle that ["every note is only an element which receives its quality only from the network of links and back-links"](https://luhmann.surge.sh/communicating-with-slip-boxes).

The only move this post makes is to cross them. Small as that move is, it is not the field's default map: the standard [survey of LLM-agent memory](https://arxiv.org/abs/2404.13501) organizes the space by memory sources, forms, and operations, and contains neither axis. We taxonomize memory systems by their plumbing, not by their cognitive geometry.

## Three crowded provinces

**Knowledge-major, node-major** is the deployed default: the agent writes prose to itself. Karpathy's [llm-wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) is its manifesto — "the wiki is a persistent, compounding artifact" — and a [first systematic study of filesystem memory](https://arxiv.org/abs/2607.26637) (July 2026; a first observation, not settled science) documents that deployed practice has converged on exactly this form: a directory of Markdown files the agent reads, writes, and reorganizes.

**Knowledge-major, link-major** is the Zettelkasten quadrant, and it is filling fast. [A-MEM](https://arxiv.org/abs/2502.12110) links each new memory note to related ones and revises old notes as new ones arrive — and its gains concentrate on multi-hop questions, exactly where topology should matter. [HippoRAG](https://arxiv.org/abs/2405.14831) builds an explicit knowledge graph and retrieves by spreading activation over it, beating state-of-the-art RAG by up to 20% on multi-hop QA; Microsoft's [GraphRAG](https://arxiv.org/abs/2404.16130) organizes a whole corpus into an entity graph with community summaries. Crowded, and contested — more on the contest below.

**Tool-major, node-major** is the scripts folder: a flat library of reusable procedures. [Memp](https://arxiv.org/abs/2508.06433) distills agent trajectories into step-level instructions and script-like abstractions, and finds that procedural memory built by a strong model transfers to weaker ones. Most people would file Voyager here too. I think that's a misfiling, and the misfiling is instructive.

## The interesting quadrant

[Voyager](https://arxiv.org/abs/2305.16291) is usually summarized as "an agent with a skill library," which sounds like a folder of scripts. But read the paper's own emphasis: the library is *ever-growing*, the skills are "temporally extended, interpretable, and compositional," new skills are written in terms of earlier skills, and it is this compounding that the authors credit for the headline results — 3.3× more unique items and 15.3× faster tech-tree milestones in Minecraft, with skills that carry over to a fresh world. A library whose entries call each other is not a folder. It is a dependency graph, and the value has migrated into the topology of calls.

[DreamCoder](https://arxiv.org/abs/2006.08381) ([PLDI 2021](https://doi.org/10.1145/3453483.3454080)) is the cleanest formal picture of that migration I know. Its wake phase solves problems; its sleep phase compresses recurring sub-solutions into named library concepts *defined in terms of earlier concepts* — rediscovering map and fold, then building sorting out of them; starting from vector primitives and growing toward physical laws — and each addition makes the next round of search tractable. This is memoized procedural cognition. Each library concept is a stored inference: the tool-world twin of the claim I made about links — [a link memoizes a past act of judgment](/thoughts/links-as-stored-inference/); a library concept memoizes a past act of problem-solving. And [ToolLLM](https://arxiv.org/abs/2307.16789) shows the retrieval side scales, planning compositions over 16,464 real REST APIs with depth-first search over tool sequences — tool choice as graph traversal.

So the quadrant has existence proofs. What it does not have — so far as I can find — is anyone treating it as a *memory architecture for an agent's own knowledge work*. Voyager grows skills for Minecraft; DreamCoder grows concepts for program-synthesis domains; ToolLLM navigates other people's APIs. Nobody has built the agent whose accumulated understanding of a domain is a growing graph of composable operations —

```text
scrape() → extract_entities() → resolve_entities() → build_graph() → detect_change() → notify()
```

— and then compared that mind against the wiki mind, the concept-graph mind, and the scripts-folder mind on the same task stream under matched conditions. Here is the speculative part, stated as speculation: I suspect this architecture has a qualitatively different relationship to staleness. Prose describes the world as it was at write time; a pipeline re-derives its description on demand, so re-running it *is* refreshing it. If that's right, the fourth cell isn't merely another storage format — it's the natural substrate for living knowledge. But nothing published tests this.

## Reading the map

Two cautions before anyone mounts the expedition. First, the grid's discreteness is engineered, not discovered: an artifact either executes or it doesn't, but actual minds will be mixtures, and the best design points may be couplings between cells rather than pure corners. Second, eager structure has to earn its keep. Microsoft's own [LazyGraphRAG](https://www.microsoft.com/en-us/research/blog/lazygraphrag-setting-a-new-standard-for-quality-and-cost/) matched or beat full GraphRAG at 0.1% of the indexing cost by deferring graph construction to query time — the sharpest published warning that pre-built topology is often not worth building. The tool-composition quadrant will face the same audit: an eagerly grown skill graph versus pipelines composed on demand.

That audit is exactly why I want the map. It converts "which memory system is best?" — a question today's benchmarks can't cleanly answer anyway — into "which regions of a small design space have we actually visited, under controls?" Three provinces have products, papers, and vendor wars. The fourth has a Minecraft agent, a program-synthesis system from 2021, and silence.

**What would change my mind:** A matched-conditions comparison — same base model, same task stream — in which the four quadrants produce no reliable behavioral differences would tell me the axes are cosmetic and the map decorative. A LazyGraphRAG-style result for tools, where on-demand composition matches an eagerly grown skill graph at a fraction of the cost, would tell me the empty cell is empty because it deserves to be. And a pointer to prior work that already treats tool-composition graphs as a general memory architecture would delete "unexplored" from my map — a correction I would welcome.
