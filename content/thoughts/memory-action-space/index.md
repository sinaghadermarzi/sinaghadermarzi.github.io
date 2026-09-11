---
title: "Memory Is an Action Space"
# draft: true
date: 2026-08-23
description: "Two agents over the identical Markdown files are different minds if they hold different tools — an argument that a memory is defined by its operations, not its bytes."
---

Take one directory of Markdown notes and hand it, byte for byte, to two agents. The first gets exactly one operation: grep. The second gets grep, plus the ability to follow the wiki-links between pages, plus semantic search, plus a shell in which any script it finds can be executed. We would casually say the two agents "have the same memory." I think that sentence is false in every way that matters. They share a *store*. The first agent can recall whatever happens to be phrased the way it was written down. The second can reach conclusions that no single page contains, and can re-activate knowledge that exists only as behavior. Same files, different minds.

## The store is not the architecture

We name memory systems after their storage representation — "a vector store," "a knowledge graph," "a Markdown wiki" — as if the noun settled the question. It doesn't. The representation fixes what could, in principle, be known. What the agent can actually *do* with the store is fixed by the operations it holds over it: read, grep, embed-and-search, follow a link, append, diff, execute.

Credit where due: this is not my observation. [CoALA](https://arxiv.org/abs/2309.02427), the standard academic frame for language agents, defines an agent by its memories *and* its action space, with internal actions — retrieval, reasoning, learning — ranking equally with external ones. And [MemGPT](https://arxiv.org/abs/2310.08560) built the point into engineering practice back in October 2023: memory operations are function calls the model itself issues, paging information between the context window and external stores. The push I want to make is only one notch further: the action space is not plumbing *around* the memory. It is half of the memory's identity.

$$
\mathcal{A} = (M,\ \mathcal{O}), \qquad K(\mathcal{A}) \;=\; \bigcup_{k \ge 0} \{\, (o_k \circ \cdots \circ o_1)(M) \ :\ o_i \in \mathcal{O} \,\}
$$

In words: a memory architecture is a store together with an operation set, and the knowledge available to the agent is everything reachable by *composing* those operations over that store. Change the operation set and you change the reachable knowledge — without touching a byte.

The composition part is what the two-agent experiment makes vivid. Roughly:

| Operation | What it makes cheap | What its absence forecloses |
|---|---|---|
| `grep` | exact-phrase recall | anything phrased differently than it was stored |
| semantic search | paraphrase recall | content sharing no surface strings with the query |
| link traversal | multi-hop association | connections nobody wrote down on any one page |
| execution | re-running procedures | knowledge that exists only as behavior |

The last row is the one that changes kind rather than degree. A script's content is not its text; it is what happens when it runs. An agent that can read `simulate.py` but not execute it holds a memory of prose *about* a capability, not the capability.

## A first measurement

Until recently this was an argument by thought experiment. Then a July 2026 preprint — the [first systematic study of filesystem-based agent memory](https://arxiv.org/abs/2607.26637), the deployed default of a Markdown directory that the agent itself reads, writes, and reorganizes through generic file tools — reported something I find remarkable: "changing the tool set alone reshapes the store as strongly as swapping the model." The tool set functions, in the authors' phrase, as a structural lever. The shape of the accumulated store turns out to be a signature of the management model *and its tools*, not of the content that flowed in.

Notice what that measures. Everyone would accept without argument that swapping the model changes the mind. The finding is that the operation set is a lever of the same order — and that it acts on the *write* side too: the tools don't just gate what can be read out of a store, they sculpt the store that gets built in the first place. Every hedge applies: this is one paper, one experimental setting, weeks old, unreplicated. It is a first measurement, not an established phenomenon. But a first measurement pointing exactly where the thought experiment points is worth taking seriously.

There is an industry datum with the same shape. Anthropic's memory tool is, concretely, a directory of files behind a handful of file operations; the company [reports](https://claude.com/blog/context-management) that adding the memory tool plus context editing improved an internal agentic-search evaluation by 39% (context editing alone: 29%). Self-reported, vendor benchmark, so weight it accordingly — but note the form of the intervention. Nothing about the representation changed. The operations did.

The same filesystem paper also found that organization buys retrieval economy without buying answer quality, which is a different thread of this series — I take it up in [A Beautiful Hierarchy Is Not Intelligence](/thoughts/when-organization-pays/).

## The honest counterweight

Two things cut against the strong version of this thesis, and I want them in the open.

First, operations are partly fungible given a strong enough model. An agent with grep and patience can emulate link traversal by iterated searching; today's coding agents do exactly this all day, and it works better than the table above implies. So the operation set may set the *price* of a cognitive move rather than a hard reachability boundary. My reply is that over a long-lived mind, prices are destiny: the cheap operations are the ones that actually get used, and the store grows around the habits they induce — which is presumably why the tool set leaves a measurable signature on store shape. But that extrapolation from store shape to long-run cognition is my speculation, not the paper's finding.

Second, on today's benchmarks memory systems mostly buy cost and latency, not correctness. In [Mem0's own evaluation](https://arxiv.org/abs/2504.19413) the full-context baseline out-scores the memory system on the LLM-judge metric (roughly 73 versus 68, a point [Zep's re-analysis](https://blog.getzep.com/lies-damn-lies-statistics-is-mem0-really-sota-in-agent-memory/) makes loudly). If the action space were as decisive as I'm claiming, shouldn't correctness gaps be larger by now? My reading is that current benchmarks are short-horizon and their histories small enough to stuff into context, so operational differences surface only as economics. The action-space thesis is really a thesis about long horizons, where store shape and operational habit compound — and stated that way, it is a hypothesis awaiting exactly the kind of longitudinal evidence the field doesn't yet collect. I sketch what that measurement program would look like in [The Missing Science of Semantic Memoization](/thoughts/science-of-semantic-memoization/).

There is also a confound worth naming: nearly every published comparison varies store and operations *together*. [HippoRAG](https://arxiv.org/abs/2405.14831) adds a graph and a PageRank-style walk over it in one move, and wins multi-hop retrieval by up to 20% while running 10–20× cheaper than iterative retrieval — but you cannot tell from that how much is the graph and how much is the walk. That entanglement is precisely why the filesystem result matters: it is, as far as I can find, the first controlled turn of the operations dial with the store held fixed.

Where this goes next is the question of what gets persisted at all — prose that describes, or programs that do, in the spirit of [Voyager's](https://arxiv.org/abs/2305.16291) executable skill library — which deserves its own essay: [Markdown Minds and Tool Minds](/thoughts/markdown-minds-and-tool-minds/). For now the claim is narrower. When you describe an artificial mind, the file listing is its anatomy. The tool list is its physiology.

**What would change my mind:** A replication of the tool-set experiment in which the effect shrinks toward noise as model capability grows — strong models emulating absent operations so cheaply that the action space washes out of both store shape and downstream answers — or a long-horizon evaluation where a grep-only agent matches the full-toolkit agent over the same store at equal token budget. Either result would demote the operation set from half of a mind's identity to a rounding error in its costs.
