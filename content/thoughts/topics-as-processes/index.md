---
title: "A Topic Is a Process, Not a File"
# draft: true
date: 2026-06-11
description: "Give a note a refresh interval and a watch list and it stops being a document — the systems ancestry of living topics, and the one thing about them that is actually new."
---

Take any note in your knowledge base — say, the one holding what you currently believe about agent memory — and add a few lines of front matter:

```yaml
topic: semantic-memory-architectures
last_reviewed: 2026-08-22
volatility: high
refresh_interval: 30d
watch_for:
  - empirical comparisons of memory architectures
  - stale-knowledge detection
notify_if: a new result changes a conclusion in this page
```

`last_reviewed` is ordinary metadata; it describes the past. The other fields are different in kind. `refresh_interval` is a promise about the future. `watch_for` is an obligation to keep looking. `notify_if` is a condition that must be re-evaluated against a world that keeps moving. The moment those fields exist, the note has commitments — and files do not honor commitments. Processes do.

## The page is a projection

That is the claim of this post: the natural unit of a living knowledge base is not the page but the persistent process behind it. The process has state — a current synthesis, its provenance, its unresolved questions, its freshness — and policy: what to watch, where to look, when to wake, when to speak. The Markdown page is the process's human-readable projection, the way `ps` output is a projection of a running program.

$$
\text{topic} = (\sigma,\ \rho,\ \nu,\ \tau,\ W,\ N)
$$

In words: a topic is a current synthesis ($\sigma$) with an evidence trail ($\rho$), an estimate of how fast its ground truth moves ($\nu$), a cadence for looking again ($\tau$), a watch predicate ($W$) over the world's stream of changes, and a notification policy ($N$) deciding which detected changes are worth a human's attention. The page renders $\sigma$. Everything else is the daemon.

The file layer of this idea is already mainstream. Karpathy's [LLM-wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) sells the wiki as much on its maintainer as on its files — "LLMs don't get bored," he notes, of the thing that keeps it current — and tirelessness is a virtue of processes, not of documents. Even the file formats are drifting toward process semantics: Google's [OKF v0.2](https://cloud.google.com/blog/products/data-analytics/okf-v0-2-adds-trust-signals) added a `stale_after` field so that "staleness becomes a plain date comparison" — though I should say plainly that OKF is a weeks-old v0.x spec with unknown adoption, a hint about where things are heading rather than a standard.

## Nineteen ninety-two called

I want to be honest about the ancestry here, because almost every component of that tuple has a name and a date, and naming them makes the idea stronger, not weaker.

The watch predicate is a *standing query*. [Tapestry](https://dl.acm.org/doi/10.1145/141484.130333) introduced continuous queries over an append-only message store at SIGMOD 1992: a user's one-shot query is rewritten into "an incremental query that efficiently finds new matches" as new documents arrive. [NiagaraCQ](https://dl.acm.org/doi/10.1145/335191.335432) scaled the idea to millions of standing queries in 2000 — explicitly framed as the machinery that would "transform a passive web into an active environment" — and it scaled by grouping similar queries so they share computation, which is also the answer to the obvious objection that per-person topic daemons can't possibly be affordable: your topic and mine, on the same subject, should share most of their monitoring.

The synthesis is a *materialized view*. A topic page is an expensive query result persisted for reuse, and updating it when the underlying world changes is incremental view maintenance — a problem with [a whole taxonomy since 1995](https://pubs.dbs.uni-leipzig.de/se/node/395) and a commercial ecosystem today: [Materialize](https://materialize.com/blog/ivm-database-replica/) literally sells "your query is a persistent process."

And the cadence fields are *crawler theory*. [Cho and Garcia-Molina](https://dl.acm.org/doi/10.1145/335191.335391) formalized freshness scheduling for web crawlers around 2000: per-source change models, optimal revisit policies under a fixed budget. My `volatility: high / refresh_interval: 30d` is a crawler refresh policy applied to concepts instead of URLs — and their math is reusable as-is. Nor is cadence a config detail you set once: evidence-based medicine, which runs living syntheses with humans as the compute substrate, is [still actively debating how often a living guideline should update](https://link.springer.com/article/10.1186/s12961-022-00866-7). Refresh cadence is a contested design variable wherever living knowledge exists.

So the skeleton is thirty years old. What, exactly, is new?

## The predicate grew a mind

Every ancestor's standing predicate was structural. Tapestry matched keywords and senders. A materialized view's merge function is relational algebra. A crawler's change detector is, at bottom, a checksum. The increment that LLMs bring — and I think it is a real one — is that **the standing query's predicate, and the view's merge function, can now be arbitrary language-level cognition**. "Wake when a new document matches these keywords" becomes "wake when something out there changes a conclusion in this page." The watch condition is defined against the synthesis itself, not against surface features of sources. The merge is not tuple insertion but re-synthesis under provenance. No 1992 system could express that predicate at any price; now it is an API call.

That one change ripples through every field of the YAML. What the semantic diff should actually compute — what is genuinely new, what got stronger, what now contradicts an old belief — is its own design problem, which I take up in [Knowledge That Stays Alive](/thoughts/living-knowledge-bases/). Whether a detected change deserves to interrupt a human is a second problem, with a forty-year HCI literature behind it, covered in [Remembering What You Care About](/thoughts/remembering-what-you-care-about/). And when the watching itself requires budgeted evidence-gathering rather than a scheduled poll, the topic process shades into the fleet of [micro-researchers](/thoughts/micro-researchers/) I've written about before — a topic is roughly what such a fleet maintains between wakings.

Is there evidence any of this pays? Early and thin, but pointed the right way. [Sleep-time compute](https://arxiv.org/abs/2504.13171) — letting an agent think about its persistent context while idle — cut test-time compute about 5x at matched accuracy and lifted accuracy up to 13–18% in Letta and Berkeley's experiments; the caveat is that this is one group's result on synthetic stateful math benchmarks, so treat it as a first observation, not an established phenomenon. Still, it is the first measurement I know of showing that a persistent context doing work *between* queries is worth paying for. And the agent-as-process framing has been in the air since [MemGPT](https://arxiv.org/abs/2310.08560) cast the LLM as an operating system with memory tiers and interrupts in 2023; topics-as-processes just pushes that framing down one level, from the agent to each thing the agent knows about.

## A daemon for meaning

Background processes got their name at MIT's Project MAC in the early 1960s. Corbató is [widely quoted](https://en-academic.com/dic.nsf/enwiki/256760) as recalling that his team "fancifully began to use the word daemon to describe background processes that worked tirelessly to perform system chores" — the allusion being to Maxwell's demon, the imaginary being that watches a stream of molecules and sorts them, tirelessly, by whether they matter.

That is the right image to end on. A topic daemon is a Maxwell's demon for meaning: it sits on the world's stream of changes and sorts them into the few that alter what you believe and the many that don't. And the physics carries a warning worth keeping: Maxwell's demon does not work for free — it pays for its sorting in measurement and memory. So will ours. The question of which topics deserve a demon at all is an economic one, and it belongs to the same ledger as everything else in this series.

**What would change my mind:** A lazy baseline winning — if regenerating the synthesis on demand from raw sources matches maintained topic-processes on freshness and correctness at comparable total cost, the daemon is overhead and a cached file plus a cron job was enough. I would also downgrade the thesis if idle-time results like sleep-time compute fail to replicate outside synthetic benchmarks, or if shared-computation tricks in the NiagaraCQ tradition turn out not to transfer to language-level predicates, leaving per-topic monitoring permanently uneconomic.
