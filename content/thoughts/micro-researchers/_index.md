---
title: "Mircro Researchers: Research as a Standing Process"
---
# Micro-Researchers: Research as a Standing Process

Ask a deep-research product a hard question and it does something genuinely impressive: it plans, browses, backtracks, reads a few hundred pages, and hands you a cited report about twenty minutes later. Then it forgets the question ever existed. Ask again next month and it starts from zero, re-crawls the same sources, and rebuilds the same bibliography — a bibliography that, measured across deployed systems, carries [citation hallucination rates of 11–57%, with 3–13% of cited URLs fabricated and 5–18% broken](../docs/10-frontier.md). These systems are also [unreliable at revising their own reports across turns](https://arxiv.org/pdf/2601.13217), so you can't even hand the old report back and ask for an update.

The error rates will improve. What interests me is the shape of the thing: research compressed into a burst, then amnesia. Because almost none of the questions I actually want researched are twenty-minute questions.

## Real questions mostly wait

"Is this dependency quietly dying?" "Does that vendor's benchmark claim survive independent replication?" "Will the company that has been advertising an impossible role for eight months eventually relax the requirements?" — the requirement-retraction question from [the first post in this series](./01-the-resolution-of-the-instrument.md) is exactly this kind of question. None of these can be answered by reading harder today. The evidence doesn't exist yet. It arrives on the world's schedule — a release, a retraction, a follow-up paper, an edited job posting — over weeks and months.

Which means real research is mostly *waiting*, punctuated by small observations. The deep-research products automate the reading and discard the waiting, and the waiting was the hard part: keeping the question alive, remembering what would count as evidence, noticing when it shows up. A twenty-minute burst is a snapshot. Most research questions need a process.

## A researcher the size of a sensor

So here is the object I keep wanting to exist. Call it a **micro-researcher**:

$$
R=(H,E,S,B,U,\Pi)
$$

In words: a hypothesis ($H$); a schema of what counts as admissible evidence ($E$); a search-and-observation strategy ($S$); a resource budget ($B$); a belief-update rule ($U$); and an escalation policy ($\Pi$) that says when the accumulated evidence is worth a report, a revision, or a human's attention. Its job is to maintain the evidential state of one proposition over time, under a small budget.

The crucial design commitment is that most of the time a micro-researcher should do *nothing*. Its lifecycle is closer to a sensor than to an agent:

$$
\text{sleep} \rightarrow \text{trigger} \rightarrow \text{cheap observation} \rightarrow \text{update} \rightarrow \text{sleep}
$$

It sleeps until a timer or an event fires; it makes one cheap observation — has the posting changed, did the repository see a commit, did the preprint get a version 2; it updates its evidential state; it goes back to sleep. For the job-posting hypothesis: $H$ is "this requirement will be retracted before the role is filled," $E$ admits dated snapshots of the posting, $S$ is a weekly diff, $B$ is a few cents, $U$ increments belief on each observed relaxation, and $\Pi$ escalates when belief crosses a threshold. Nothing in that loop needs a frontier model.

## Three disciplines, one of them seventy years old

Three disciplines keep this from degenerating into a fleet of newsletter summarizers.

**Structured evidence, not rewritten narratives.** The researcher's state is a typed log of observations with provenance and timestamps — not a prose summary it re-synthesizes on every waking. Narratives rot: each rewrite loses provenance, and the [multi-turn revision result](https://arxiv.org/pdf/2601.13217) says models are bad at exactly this operation. The [memory literature's hard-won lesson](../docs/11-agent-memory.md) points the same direction: append-only, auditable, diffable state beats cleverly rewritten stores.

**Active falsification.** A micro-researcher should seek the observation most likely to *kill* its hypothesis, and track competing explanations, rather than accumulate confirmation. An agent that only collects supporting evidence is a clipping service.

**Schedule by expected information gain per unit cost.** When budget is scarce, the next observation to buy is the one with the highest expected reduction in uncertainty per dollar. I want to say plainly that this formal core is seventy years old: it is [Lindley's 1956 criterion](https://projecteuclid.org/journals/annals-of-mathematical-statistics/volume-27/issue-4/On-a-Measure-of-the-Information-Provided-by-an-Experiment/10.1214/aoms/1177728069.full) — choose the experiment that maximizes expected information — carried forward by [modern Bayesian optimal experimental design](https://arxiv.org/abs/2302.14545) and instantiated in machine learning as [active learning](https://burrsettles.com/pub/settles.activelearning.pdf): query what is most informative per label cost. I am not proposing new mathematics. The proposal is packaging: BOED as the management layer for a fleet of persistent LLM agents, where a language model — not a statistician — writes down $H$, $E$, and $U$ for each hypothesis.

## Big models theorize, cheap models watch

The economics only work if the fleet is stratified. Modern computation offers a **capability–cost spectrum**:

$$
\text{frontier LLM} \rightarrow \text{medium LLM} \rightarrow \text{small LLM} \rightarrow \text{specialized model} \rightarrow \text{SQL/statistics} \rightarrow \text{deterministic code}
$$

and the standing scheduling question, for every subproblem, is: *what is the cheapest executor capable of performing this subproblem with sufficient reliability?* A large model earns its price twice: at the top, decomposing a vague theory into falsifiable hypotheses and dispatching narrow, inexpensive researchers; and occasionally, when anomalies accumulate — revising theories, merging hypotheses, reallocating budgets. Everything in between is cheap observation: a small model reading a diff, a SQL query counting postings, a script polling a feed.

What emerges is a *portfolio* of hypotheses under active management: fund the ones whose next observation is cheap and informative, pause the ones waiting on the world, merge the ones that turn out to be the same claim, branch the ones that fork, terminate the ones falsified or no longer decision-relevant — all by expected information value per unit cost. This too has honest ancestors — including, I should concede, the economics. Gittins indices and multi-armed bandits are the operations-research version of "allocate attention across uncertain projects"; 1990s [BDI agent architectures](https://cdn.aaai.org/ICMAS/1995/ICMAS95-042.pdf) had persistent beliefs with event-triggered deliberation; and Dean and Boddy's [deliberation scheduling](https://www.semanticscholar.org/paper/Deliberation-Scheduling-for-Problem-Solving-in-Boddy-Dean/31585092a9fe9d15e0e0c1ff030adca9a023cb2f) — allocating computation across anytime algorithms "based on performance expectations," later [compiled by Zilberstein into utility-maximizing compositions of quality-cost-profiled modules](http://rbr.cs.umass.edu/shlomo/papers/Zshort93.pdf) — was budgeted portfolio management of computation in 1988. What that work assumed was a fixed menu of algorithms with known performance profiles. Here each portfolio entry is an open-ended hypothesis whose expected-information profile nobody hands you: a language model has to estimate it from the current state of the evidence, and revise it every time the world moves.

## The neighbors: living reviews, robots, bursts, and sleep

Almost every piece of this exists somewhere — and in a couple of fields, most of the loop already runs. What doesn't exist is the general form.

**Medicine runs the loop with humans in it.** A Cochrane [living systematic review](https://journals.plos.org/plosmedicine/article?id=10.1371/journal.pmed.1001603) is "a systematic review that is continually updated, incorporating relevant new evidence as it becomes available" — continual surveillance of the literature, with explicit [criteria for when a question deserves living mode and when new evidence warrants an update](<https://www.jclinepi.com/article/S0895-4356(17)30636-4/fulltext>). That is the micro-researcher lifecycle operating today, with editorial teams as the executors and update triggers as policy. Pharmacovigilance goes further: adverse-event databases are [rescanned on a schedule with disproportionality statistics](https://arxiv.org/html/2604.18898v1), every drug–event pair a dormant hypothesis whose evidential state cheap statistics keep updating until a "signal" crosses threshold and escalates to expensive expert review — sleep, trigger, observe, update, escalate, running for decades at the scale of millions of hypotheses. What both hard-code is the schema: one kind of hypothesis, one kind of evidence, set by the institution. The open part is the generalization — hypotheses a language model writes down, heterogeneous evidence, budget moving across a portfolio.

**The Robot Scientist closed the loop in hardware, twenty years ago.** King's Adam [originated functional-genomics hypotheses, physically ran its own experiments — about a thousand a day — and falsified the hypotheses inconsistent with the data](https://www.nature.com/articles/nature02236), choosing experiments to discriminate between hypotheses at least cost; twelve of its novel enzyme hypotheses were confirmed, and the follow-up paper was titled, accurately, ["The Automation of Science"](https://www.science.org/doi/abs/10.1126/science.1165620). Its successor Eve pointed [active learning at drug screening, where compounds cost more than ten times the price of gold](https://royalsocietypublishing.org/rsif/article/12/104/20141289/35592/Cheaper-faster-drug-development-validated-by-the), and surfaced an antimalarial lead. That is information-per-unit-cost as working machinery — in one closed lab domain, with a fixed ontology, never dormant. The micro-researcher is Adam's discipline without Adam's lab: open-world textual evidence, and mostly asleep.

**Deep research agents** proved the capability. [OpenAI's Deep Research](https://openai.com/index/introducing-deep-research/), RL-trained end-to-end on browsing tasks, scored 26.6% on Humanity's Last Exam against roughly 9% for o1 — a real jump, documented alongside its citation pathologies in [the review's frontier doc](../docs/10-frontier.md). But it is burst-shaped by construction: one question, one spike of compute, one disposable report.

**Google's AI co-scientist** is the most serious LLM research architecture yet shipped: generation, reflection, and ranking agents whose hypotheses compete in [Elo-rated tournaments](https://research.google/blog/accelerating-scientific-breakthroughs-with-an-ai-co-scientist/), with wet-lab-validated results — drug-repurposing candidates for acute myeloid leukemia confirmed in vitro, later a [Nature paper](https://www.nature.com/articles/s41586-026-10644-y). It has the portfolio instinct — hypotheses competing under a selection rule — but it is burst-mode too: enormous test-time compute at a single point in time, not evidence accruing from cheap scheduled observations over months.

**Ambient agents** are the substrate without the epistemics. LangChain's [ambient agents](https://www.langchain.com/blog/introducing-ambient-agents) "listen to an event stream and act on it accordingly," escalating to humans only at important junctures — motivated by the observation that chat "limits the ability of humans to scale themselves." [ChatGPT's scheduled tasks](https://help.openai.com/en/articles/10291617-scheduled-tasks-in-chatgpt) and Claude Code's routines productize the sleep–trigger–act loop. The machinery ships today. But these products schedule *tasks*; a micro-researcher schedules *hypotheses* — the difference is the belief state carried between wakings and the information-gain rule deciding which waking is worth paying for.

**Letta's [sleep-time compute](https://www.letta.com/blog/sleep-time-compute/)** is the same insight aimed inward. Agents think while idle, reorganizing memory and precomputing inferences — [~5x less test-time compute at equal accuracy](https://arxiv.org/abs/2504.13171) — and the pattern has been productized as offline consolidation across the major assistants; in ChatGPT's memory system it produced [the largest measured product-memory gain on record](../docs/11-agent-memory.md): time-sensitive memory accuracy rising from 9.4% to 75.1%, from consolidation rather than retrieval. Point that idle cognition outward — at the world instead of the context window — and you have a micro-researcher. And [FutureSearch](https://futuresearch.ai/), whose agent fleets maintain probabilistic answers to standing questions, is the nearest operational existence proof that belief-maintenance-as-a-service can work.

## What would make it real

Three ingredients, all of them trending the right way.

*Cheap observation.* The economics that killed this idea in every previous decade are collapsing: [GPT-3.5-level MMLU performance fell from $20 to $0.07 per million tokens in about 18 months](https://epoch.ai/data-insights/llm-inference-price-trends). A hypothesis that needs a few cents of machine judgment per week can now exist at thousands of instances — the class of system that was economically impossible when every recurring judgment required a human.

*Durable structured state.* A researcher that lives for months needs memory that survives and can be audited. The [memory review](../docs/11-agent-memory.md) says the boring answer is the right one: provenance-tagged files, append-only evidence logs, scheduled consolidation — and treat every memory write as untrusted input, because a poisoned observation is a poisoned belief.

*Delegation as a capability.* Someone has to decide that this hypothesis deserves a watcher, that a small model suffices for this observation, that these two researchers should merge. That is precisely the **fourth capability** — decomposition, capability matching, budget allocation, verification — argued in [the second post in this series](./02-delegation-is-a-capability.md). The underlying bet — that serious epistemic work survives being factored into small tasks executed by agents that never see the whole problem — is [Ought's factored cognition hypothesis](https://ought.org/research/factored-cognition), and their argument for [supervising process rather than outcomes where outcomes are out of reach](https://ought.org/updates/2022-04-06-process) describes the standing-question regime exactly: the outcome arrives on the world's schedule, so the process is all there is to supervise. A micro-researcher fleet is delegation running as a standing process: the orchestrator's **theory of capability** decides who watches, and its budget rule decides how often.

And underneath all of it sits the principle this series started with. The **Evaluative Efficiency Principle** from [the first post](./01-the-resolution-of-the-instrument.md) says an evaluation is justified only when the expected decision-relevant information it adds exceeds the full cost of obtaining it. A micro-researcher is that principle compiled into a daemon: an evaluation that knows when it isn't worth running. The [evaluation literature](../docs/08-evaluation-and-evidence.md) is one long catalog of what happens when measurement runs cost-blind — the deepest thing a research process can know about itself is that, right now, the most informative action per dollar is to sleep.

## If you want to go deeper

- [The frontier doc](../docs/10-frontier.md) — deep research agents and their citation-hallucination record, the co-scientist, and why capability follows verifiability.
- [The agent-memory doc](../docs/11-agent-memory.md) — sleep-time compute, consolidation-beats-retrieval, files-first memory, and memory poisoning.
- [The evaluation doc](../docs/08-evaluation-and-evidence.md) — the cost-aware measurement discipline this architecture presumes.
- [Lindley (1956)](https://projecteuclid.org/journals/annals-of-mathematical-statistics/volume-27/issue-4/On-a-Measure-of-the-Information-Provided-by-an-Experiment/10.1214/aoms/1177728069.full), [Modern Bayesian Experimental Design](https://arxiv.org/abs/2302.14545), and [Settles' active learning survey](https://burrsettles.com/pub/settles.activelearning.pdf) — the seventy-year-old formal core.
- [The prior-art map](./05-prior-art-map.md) — the full accounting of neighbors, from living reviews and pharmacovigilance to deliberation scheduling, BDI architectures, and ambient agents.
