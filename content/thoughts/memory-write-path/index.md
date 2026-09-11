---
title: "The Write Path"
draft: true
date: 2026-08-23
description: "Agent memory research pours everything into retrieval while the decision of what to write goes unexamined — and the early evidence says a bad write cannot be rescued later."
---

Open any recent agent-memory paper and count where the effort goes. [LongMemEval](https://arxiv.org/abs/2410.10813) decomposes the memory problem into indexing, retrieval, and reading — three stages, every one of them on the read path — and shows that optimizing each stage measurably helps. The leaderboards score whether the right memory comes back. The vendor benchmark wars are fought over recall. Meanwhile the event that made any of this possible — something was *written*, by some policy, in some representation — goes almost entirely unexamined. Secondary analyses of [MemoryAgentBench](https://arxiv.org/abs/2507.05257) report that Mem0's memory construction costs roughly 20,000 times what BM25 indexing does: the write path is where the money goes, and it is not even scored.

The asymmetry made sense for classic RAG, where the corpus is handed to you and the only decision is how to search it. A long-lived agent has no given corpus. It writes its own. The real loop is: experience → decide what is worth preserving → choose a representation → write, link, or compress → much later, retrieve → possibly revise. Retrieval is the last step of a pipeline whose first steps nobody instruments.

## A bad write cannot be rescued

The claim I want to defend is simple: admission — the decision of what gets written, and how — matters as much as retrieval, because retrieval can only choose among what admission preserved. However clever your reranker, it is searching the residue of earlier write decisions. And a hallucination that clears admission stops being a transient mistake; it becomes part of what the agent knows.

The direct evidence is young but points one way. The clearest result is the [experience-following study](https://arxiv.org/abs/2505.16067) from May 2025: when a new task resembles a stored record, the agent's output follows the stored output — for better or worse. Bad records don't just sit there; they propagate, compounding over the deployment lifetime, while outdated records steer current tasks wrong. The fix was not better retrieval. Selective admission plus selective deletion yielded roughly ten points of absolute improvement over naively writing everything down.

A second, sharper datum comes with a caveat I want to state as loudly as the number. A [July 2026 preprint](https://arxiv.org/abs/2607.21962) on longitudinal memory evaluation found that weakly written memory facts failed downstream about 24% of the time versus roughly 2% for well-written ones — a twelvefold difference attributable to write quality alone. That is my thesis in one number, which is exactly why I distrust it: it is an unreplicated preprint whose benchmark runs on fully synthetic "life-script" data. A single data point, in other words. But its direction agrees with everything else here.

Third: the write policy is *learnable*, and cheaply. [Memory-R1](https://aclanthology.org/2026.acl-long.583/) (ACL 2026) trains a memory manager to choose ADD, UPDATE, DELETE, or NOOP by reinforcement learning, rewarded only by downstream answer quality — and from just 152 training pairs it generalizes across three benchmarks and model scales from 3B to 14B. The motivating failure is telling: told "I adopted Buddy" and later "I adopted Scout," heuristic write pipelines delete or fragment the first fact; the learned policy consolidates them into "adopted two dogs." (Honesty requires noting the gains are anchored on LoCoMo, a benchmark whose [audited answer key is 6.4% wrong](https://penfieldlabs.substack.com/p/we-audited-locomo-64-of-the-answer), judged by an LLM that accepts most intentionally wrong answers.) [Mem-α](https://arxiv.org/abs/2509.25911) pushes the same idea further: a write policy trained on episodes under 30k tokens generalizes to histories over 400k — thirteen times its training length. Write-time investment pays on the way back out, too: [MemInsight](https://arxiv.org/abs/2503.21760) (EMNLP 2025) has the agent attach structured attribute annotations at admission and reports up to 34% better retrieval recall than a standard RAG baseline.

Even maintenance is a write. The [filesystem-memory study](https://arxiv.org/abs/2607.26637) found that an agent allowed to reorganize its own store "silently condenses content unless one preservation rule is added" — destruction by tidying, and the model's own default tendency rather than anything it was asked to do.

That is close to the whole direct evidence base: roughly three papers, one synthetic-data preprint, and an attack I'll get to next — young, unreplicated, so far uncontested. It is why I say the write path *may* matter as much as the read path, and why the per-term instrumentation to settle it belongs to [the empirical science of memory that doesn't exist yet](/thoughts/science-of-semantic-memoization/).

## One record in a thousand

If bad writes merely degraded quality, admission would be an optimization problem. It is worse than that: it is a security surface. [AgentPoison](https://arxiv.org/abs/2407.12784) (NeurIPS 2024) showed that injecting optimized records into an agent's memory or knowledge base achieves about 63% end-to-end attack success at a poisoning ratio under 0.1% — one record in a thousand — with under 1% impact on benign performance and no access to the model at all. The attack exploits the same mechanism the experience-following paper documents benignly: agents trust what they retrieve from their own store far more than they should. And retrieval-side defenses are structurally late — by the time the poisoned record is being retrieved, the interesting decision already happened at admission. I made this point from the other direction in [the micro-researchers essay](/thoughts/micro-researchers/): treat every memory write as untrusted input, because a poisoned observation is a poisoned belief.

## Psychology got here in 1972

I want to be careful about what is actually new here, because the core insight is not. Craik and Lockhart's levels-of-processing framework — cognitive psychology, 1972 — argued that human retention depends on the depth of processing at *encoding*: shallowly encoded material stays fragile no matter how hard you cue retrieval later. That is the write-path thesis, stated about humans, fifty-four years before the agent papers. What is genuinely new is that in machine systems the encoding policy has become a trainable, attackable, auditable *object*: you can learn it from 152 examples, subvert it with one record in a thousand, and — soon, I hope — benchmark it separately from retrieval.

Until those benchmarks exist, I price writes with a heuristic:

$$
V_{\text{write}} \;\propto\; p_{\text{reuse}} \times c_{\text{reconstruct}} \times \text{confidence} \times \text{density} \times \text{stability}
$$

In words: a memory earns durable storage in proportion to how likely it is to be needed again, how expensive it would be to rebuild from sources, how much you trust it now, how much it says per token, and how long it will stay true. This is a heuristic, not measured science — none of the five factors has an agreed estimator, and multiplying them is a guess about how they interact. But it makes the right failure modes expensive: write everything and expected reuse per record collapses toward zero; write conclusions without provenance and confidence becomes unknowable exactly when you need it. And whatever you admit bounds what organizing can ever do for you afterward — [a beautiful hierarchy over badly chosen contents is garbage, efficiently arranged](/thoughts/when-organization-pays/).

**What would change my mind:** A demonstration that a strong retrieval-plus-verification layer recovers essentially all of the performance lost to naive admission — reading systematically rescuing bad writing at scale — would collapse the asymmetry this post is built on. So would failed replications of the experience-following or write-quality results on non-synthetic, audited benchmarks; the direct evidence base is about three papers and one attack, and my confidence is sized to that.
