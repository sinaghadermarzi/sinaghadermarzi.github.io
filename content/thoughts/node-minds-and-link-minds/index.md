---
title: "Node Minds and Link Minds"
draft: true
date: 2026-08-23
description: "Whether knowledge lives inside rich documents or in the topology between small ones is an old tradeoff that LLMs have just repriced on both sides."
---

Niklas Luhmann kept his working memory in a slip box: roughly 90,000 cards accumulated over decades, each one too small to stand alone, conventionally credited with enabling some seventy books and four hundred papers. He was explicit about where the intelligence lived: ["Every note is only an element which receives its quality only from the network of links and back-links within the system. A note that is not connected to this network will get lost in the card file and be forgotten by it."](https://luhmann.surge.sh/communicating-with-slip-boxes) The unit of knowledge was not the card. It was the path through the cards.

The essayist's notebook is the opposite instrument: long, self-contained entries, each carrying its own context and qualifications, each readable alone. Nothing important lives between the entries, because there is no between — only the next page.

I keep arriving at this same fork when I think about how a long-lived AI agent should externalize what it learns. Should its persistent knowledge be a shelf of rich documents — each a small essay that answers a question by itself? Or a dense graph of small atoms, where answering means entering somewhere and walking? Call them node minds and link minds. My claim is deliberately modest: this is a real architectural axis, it is far older than the current literature, and the honest empirical reading in 2026 is "it depends on the workload" — in a specific, increasingly measurable way.

## The oldest polarity in information systems

Credit where due, because the lineage here is deep enough to be embarrassing. Vannevar Bush drew the axis in [1945](https://www.theatlantic.com/magazine/archive/1945/07/as-we-may-think/303881/): indexes impose artificial paths, association is how minds actually move, and his memex was accordingly a machine for selection by association rather than indexing. Ted Nelson, who [coined "hypertext" in 1965](https://doi.org/10.1145/800197.806036), later insisted that everything is "deeply intertwingled" and that hierarchies of tidy self-contained subjects are "usually forced and artificial." And the sharpest version comes from database engineering, where [Codd's relational model](https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf) made the tradeoff a design decision with a name. The normalization-vs-denormalization argument transfers to agent memory almost verbatim:

|  | Node mind | Link mind |
|---|---|---|
| Database analogue | Denormalized: rich rows, redundant copies | Normalized: atomic facts, joins |
| Read | One lookup; self-contained answer | Multi-hop traversal; composed answer |
| Update | Anomalies — every page repeating a fact goes stale | Edit one atom; consistency by construction |
| Historical failure mode | Contradictions accumulate between pages | Starved by the cost of writing links |

Even the biological prior points somewhere specific. Steyvers and Tenenbaum's 2005 analysis of human semantic networks — free-association norms, WordNet, Roget's — found small-world structure everywhere: sparse graphs with average path lengths around three hops and heavy-tailed hubs. Whatever human semantic memory is, it is not a filing cabinet of essays. If you take the analogy seriously (I take it as suggestive, no more), biology voted link-major a long time ago.

So I want to be clear about what is *not* new here. The polarity is eighty years old. What is new is who does the work.

## What the LLM actually changes

Every previous generation of link-major systems died at the same spot: write cost. Bush's "trail blazers" never became a profession. The typed-link and semantic-annotation projects of every subsequent era were starved by the burden of hand-authoring relations, not refuted on the value of reading them. Luhmann is the exception that defines the rule — one unusually obsessive human paying the write cost, full time, for thirty years.

The increment of the LLM era is that the same model now sits on both sides of the trade. It is the link-writer: systems like [A-MEM](https://arxiv.org/abs/2502.12110) autonomously link each new memory to related ones, Zettelkasten-style, and evolve old notes when new ones arrive. And it is the traversal engine: [HippoRAG](https://arxiv.org/abs/2405.14831) runs Personalized PageRank over an LLM-extracted graph, which is spreading activation with a compute budget. Denormalization used to be forced on us because joins were expensive and nobody would write the foreign keys; link-writing used to be forced out because annotation was expensive. Both prices just changed at once, and nobody yet knows the new equilibrium. That is what makes this axis a live design question rather than settled history.

## A scoreboard, not a verdict

The empirical record so far is genuinely mixed, and I think the mixture *is* the finding.

The case for link minds: HippoRAG reports multi-hop retrieval gains of up to 20% over strong RAG baselines, at roughly an order of magnitude lower cost than iterative retrieval. But the gains are lopsided in an instructive way — as best I can reconstruct from its tables, around twenty recall points on 2WikiMultiHopQA, where questions decompose along clean entity links, and only about three on MuSiQue, where the hops are fuzzier. Structure pays exactly where the workload has structure.

The case against, from the same neighborhood: [Zep's temporal-graph memory](https://arxiv.org/abs/2501.13956) reports overall gains on long-horizon benchmarks while *regressing* on single-session detail recall — roughly 94.6% down to 80.4% on that category, as I read their tables. These are vendor-reported, unreplicated numbers on contested benchmarks, so hold them loosely; but the direction is the interesting part: extracting knowledge into atoms and edges is lossy compression, and the loss shows up precisely on "just tell me what the document said." [Mem0's own evaluation](https://arxiv.org/abs/2504.19413) similarly reports its graph variant adding only about two percent overall while giving ground on some hop questions. And Microsoft — having built the flagship graph-RAG system — then showed with [LazyGraphRAG](https://www.microsoft.com/en-us/research/blog/lazygraphrag-setting-a-new-standard-for-quality-and-cost/) that deferring all graph construction to query time matches or beats it at "0.1% of the costs of full GraphRAG" for indexing. Eagerly materializing the topology is often not worth it.

Yet flat retrieval has a ceiling that is now a theorem, not a vibe: DeepMind's [LIMIT paper](https://arxiv.org/abs/2508.21038) proves that for any embedding dimension there are combinations of relevant documents no single query vector can retrieve — though, honestly, that argument licenses rerankers and multi-vector retrieval as much as it licenses graphs.

Put together: eager links pay when reuse is high and the hops are clean; rich pages pay when answers need locality and verbatim detail; deferral wins when you cannot predict the queries. That is not a draw — it is a workload-dependent tradeoff, the same one database engineers have been navigating since 1970, now with cognition in the loop. It also inherits the old update anomaly: a node mind goes stale one paragraph at a time, in every page that repeats the fact — which is one reason I think [organization has to be evaluated economically](/thoughts/when-organization-pays/) rather than aesthetically.

Two agents on identical storage technology, one writing essays and one writing edges, are metabolizing the same experience into different substances. What an edge actually stores — a past act of judgment, memoized — is the subject of [the next post](/thoughts/links-as-stored-inference/); what walking the edges might buy that similarity search cannot is the one [after that](/thoughts/creativity-as-traversal/).

**What would change my mind:** a compute-matched study showing that query-time structure — LazyGraphRAG-style deferral plus rerankers — matches eagerly maintained links even on high-reuse, cleanly multi-hop workloads would collapse the link mind into a premature optimization; conversely, graph memories that stop regressing on detail recall as extraction improves would make the node mind look like a transitional form. Either result would turn this axis back into a knob.
