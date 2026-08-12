---
title: "The New Delegation Stack"
date: 2026-08-09
description: "How AI expands the space of tasks we can delegate."
---

# Delegation Is a Capability

*Part 2 of Notes on Cognitive Infrastructure.*

Somewhere in Anthropic's production stack there is a prompt that teaches one of the most capable models on Earth how to be a middle manager. It spells out staffing policy by hand: simple fact-finding gets one agent and three to ten tool calls; a direct comparison gets two to four subagents with ten to fifteen calls each; complex research gets ten or more subagents with clearly divided responsibilities ([How we built our multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system)). They wrote those rules because, without them, the lead agent did what every new manager does — early versions spawned fifty subagents for simple queries ([the review's account of the system](../docs/05-multi-agent-patterns.md)).

Sit with the asymmetry for a second. We spend staggering amounts of post-training compute teaching models to follow instructions, to reason, and to use tools — the three canonical capabilities of the modern LLM. And then, when we want a model to manage other models, we write it a memo. I've come to think the memo is a fossil in the making. Delegation is the fourth capability: it is trainable by the same recipe that produced the other three, and the 2025–26 literature shows the training has already quietly begun.

## What's hiding inside "delegate"

"Delegation" sounds like one decision. It's at least nine skills wearing a trench coat:

- recognizing when delegation is worthwhile at all (often the right answer is that it isn't),
- decomposing a problem into independently useful tasks,
- writing a good task specification for another model,
- choosing the right model, agent, or tool for each piece,
- deciding parallel versus sequential execution,
- allocating compute and budget,
- recognizing when a worker's answer is unreliable,
- deciding whether to retry, escalate, or eat the loss,
- synthesizing multiple outputs into one coherent state.

What convinces me these are real skills — and not framework plumbing — is that when multi-agent systems fail, they fail at precisely these joints. The MAST taxonomy, built from over 1,600 annotated execution traces, sorts multi-agent failures into specification issues (41.77%), inter-agent misalignment (36.94%), and task verification (21.30%), and concludes that failures stem from system design, not model limitations ([multi-agent patterns](../docs/05-multi-agent-patterns.md)). Read that as a manager's performance review: bad task specs, bad coordination, no checking of the work. "System design" is our current euphemism for the delegation skills nobody has trained yet.

## A tool is a function; another model is a distribution

Why isn't this just tool use? Because the contracts are different in kind. A calculator, a compiler, a database — each offers something close to a deterministic contract:

$$
f(x) \rightarrow y
$$

Same input, same output. You can trust it the way you trust arithmetic. Another LLM offers nothing of the sort. Delegation looks like:

$$
P(y\mid \text{agent},\; prompt,\; context,\; compute)
$$

In words: what comes back is a draw from a distribution, and that distribution shifts with who you ask, how you phrase the task, what context you supply, and how much compute the worker burns. To delegate well, the model therefore needs what I'd call a **theory of capability** — a working set of beliefs of the form *agent X is probably good enough for this subproblem; agent Y is expensive but worth invoking here; this result needs verification; these three investigations can run independently*. That makes delegation itself a substantial reasoning problem, not an API call.

The need for such a theory did not arrive with LLMs. [Dawid and Skene](https://academic.oup.com/jrsssc/article/28/1/20/6953573) built a working one in 1979 — jointly estimating the true answer *and* each unreliable annotator's error profile from agreement patterns alone, no ground truth required — and crowdsourcing platforms later ran on descendants of that machinery. Unreliable workers have always forced their delegators into statistics.

Notice what's strange about this theory compared to theory of mind: two of the conditioning variables — prompt and context — are chosen by the delegator. Part of the worker's competence is under the manager's control. I'll come back to that, because I think it's the deepest part of the whole story.

## Nobody needs a "delegate well" reward

Here is the argument that delegation is trainable, and it's short. Put a model in an environment where its action space includes *spawn agent*, *dispatch task*, *inspect result* alongside *think* and *answer*. Reward task success; penalize cost:

$$
R =
R_{\text{task success}}
-\lambda_1 C_{\text{tokens}}
-\lambda_2 C_{\text{latency}}
-\lambda_3 C_{\text{agent calls}}
$$

In words: you get paid for solving the problem, and you're docked for what you spent in tokens, in wall-clock time, and in calls to other agents. You never write a reward that says "delegate well." If delegation improves success per unit cost, outcome-based RL discovers it as an instrumental strategy — exactly how tool use stopped being a prompting trick and became a trained behavior.

Credit where due: this reward already exists in the literature, almost verbatim. [De Sabbata, Sumers, and Griffiths](https://arxiv.org/abs/2410.05563) trained language models on it in 2024, under the banner of rational metareasoning — the reward of a chain of thought is its utility minus a cost proportional to its tokens — cutting reasoning tokens 20–47% with no loss of accuracy. Their version prices *thinking*; the delegation version prices *hiring* — same ledger, other minds on it.

This is not hypothetical; the pieces are already published or shipped. [OpenAI's Deep Research](https://openai.com/index/introducing-deep-research/) showed end-to-end RL on hard browsing tasks teaches a model to orchestrate its own multi-step workflow. [Puppeteer](https://arxiv.org/abs/2505.19591) trains a centralized orchestrator with REINFORCE, cost included in the reward, and gets better performance at lower cost with pruned reasoning paths. [xRouter](https://arxiv.org/abs/2510.08439) trains a 7B router with an explicitly cost-aware RL reward that learns to answer directly, call one or more of twenty-plus downstream LLMs, decide *how to prompt them*, and synthesize the replies — the specification step, inside the action space. Moonshot's Kimi K2.5 trains its orchestrator with Parallel-Agent RL, where the orchestrator is "a learned policy, not a prompt template," sub-agent creation is an action, and the reward includes performance, parallelization, and finish-time terms — lifting BrowseComp from 60.6% to 78.4% with a 4.5x wall-clock reduction ([VentureBeat](https://venturebeat.com/orchestration/moonshot-ai-debuts-kimi-k2-5-most-powerful-open-source-llm-beating-opus-4-5)). [SearchSwarm](https://arxiv.org/abs/2606.09730) goes furthest toward naming the thing: it coins "**delegation intelligence**" and — my favorite detail in this whole literature — trains the lead agent against *deliberately weaker* subagents, so it is forced to learn tighter specification and verification. You learn to manage by being assigned bad employees. [Chain-of-Agents / AFM](https://chain-of-agents-afm.github.io/) shows the skill can even be distilled: multi-agent trajectories compressed into a single 32B model that internally simulates its own workers, then sharpened with agentic RL to 55.3% on GAIA. And at consumer scale, [GPT-5](https://openai.com/index/introducing-gpt-5/) ships a production router "continuously trained on real signals including when users switch models, preference rates, and measured correctness."

Honesty requires one caveat: no single work yet unifies that full reward — tokens, latency, and calls together — as the training signal for a general delegation capability in a frontier model. The token term exists twice over ([L1](https://arxiv.org/abs/2503.04697) trains reasoning length against a budget; the metareasoning result above prices it directly), the latency term exists (Kimi's finish-time reward), the call-cost term exists piecewise. The ingredients are all on the shelf; the synthesis is the open move.

## What the routing literature already solved — and what it didn't

I should be clear about the nearest neighbors, because they are close. [FrugalGPT](https://arxiv.org/abs/2305.05176) established the cost-quality cascade in 2023: try the cheapest model, escalate only when a learned scorer judges the answer insufficient, and match GPT-4 with up to 98% cost reduction. [RouteLLM](https://arxiv.org/abs/2406.18665) learns a router from human-preference data that predicts the probability the strong model wins on a given query, cutting cost more than 2x with no quality loss — an explicitly probabilistic model of relative capability. The "cheapest sufficient model" insight belongs to this literature, full stop.

But these routers are middleware. They sit outside the model, make one decision per query, before any work begins, over a fixed pool. The fourth-capability framing moves the allocation decision *inside* the working model, where it entangles with everything else the model is doing: how to decompose the task, how to specify each piece, whether to trust what came back, whether to buy a second opinion halfway through. A router picks a model. A delegator constructs a task, a worker, and a budget simultaneously — and revises all three mid-flight.

## Context preloading: constructing temporary minds

Now the part I find genuinely deep. A good delegator doesn't say "go research X." It practices **context preloading**: it constructs a temporary cognitive environment for the worker — deciding what the worker should know, what role it plays, what question it answers, what evidence counts, what to ignore, how much uncertainty to tolerate, and what format to return.

Picture a frontier model holding a 100,000-token research state. It realizes it needs to know whether historical counterexamples contradict hypothesis H. It does not ship the worker the whole state. It compresses everything relevant into 1,500 tokens: *here is H; here are the definitions of A and B; search specifically for counterexamples satisfying conditions X, Y, Z; do not evaluate the overall thesis; return evidence in this schema.* A model dramatically weaker than the parent now does useful work, because the parent removed most of the cognitive burden by constructing the problem correctly. Which suggests a relationship I keep returning to:

$$
\text{effective worker capability}
\approx
f(
\text{worker intelligence},
\text{quality of supplied context},
\text{task decomposition}
)
$$

In words: what you get out of a worker is not fixed by its weights. Two of the three inputs are controlled by the delegator. This means delegation itself has *quality* — two equally intelligent lead models, given identical subagents, can get radically different results because one is better at building their contexts. And it exposes a lovely asymmetry: the worker never needs to understand the whole problem. Intelligence at the top buys comprehension the bottom doesn't need.

I would love to claim that observation, but crowdsourcing researchers proved it with human workers. [Soylent](https://cacm.acm.org/research/soylent/) (2010) found open-ended editing tasks came back poor roughly 30% of the time and fixed the workers by fixing the task — the **Find-Fix-Verify** pattern of separate identification, generation, and voting stages. [Cascade](https://dl.acm.org/doi/10.1145/2470654.2466265) (2013) built coherent taxonomies from 20-second micro-judgments, at 80–90% of expert quality, with no worker ever holding a global view of the data. And the alignment lineage ran the same bet under the name [factored cognition](https://ought.org/research/factored-cognition): Ought's program, with [iterated amplification](https://arxiv.org/abs/1810.08575) behind it, asked whether hard reasoning factors into bounded-context subtasks whose agents never see the whole problem. The difference now is authorship: those decompositions were hand-designed offline, one workflow at a time; the fourth-capability claim is that the pattern language moves inside the model, synthesized per task at run time.

The production evidence lines up with this almost embarrassingly well. Anthropic's research system staffs an Opus 4 lead with Sonnet 4 subagents — cheaper workers under a smarter allocator — and beats single-agent Opus 4 by 90.2% on their internal research eval, with the engineering lessons reading like a delegation curriculum: give each subagent an objective, an output format, tool guidance, and explicit boundaries, or you get duplication and gaps ([multi-agent patterns](../docs/05-multi-agent-patterns.md)). The review's own reframe of subagents points the same way: their benefit is not persona specialization but *context isolation* — disposable windows that absorb the mess of search and return condensed 1,000–2,000-token summaries to a clean main thread ([context engineering](../docs/06-tools-memory-context.md)). And every [Claude Code subagent file](https://code.claude.com/docs/en/sub-agents) — a Markdown-and-YAML bundle of role, rules, tool grants, and a per-agent model choice — is a hand-built temporary cognitive environment. Humans are currently authoring these by hand, which is exactly how you know the job exists and whose job it eventually becomes.

## The economics of intelligence

Pull the threads together and the top-level model stops looking like a reasoner and starts looking like a **compute allocator**. Anthropic's telemetry makes the point brutally: agents use roughly 4x the tokens of chat, multi-agent systems roughly 15x, and in their BrowseComp analysis token usage *alone* explained 80% of performance variance ([the full ledger is in multi-agent patterns](../docs/05-multi-agent-patterns.md)). Orchestration performance is, to first order, a spending problem. The learned policy that matters is no longer just *how do I solve this?* but *how should I spend computation to solve this?*

I can't pretend that question is new. It is the founding question of decision-theoretic metareasoning — a formal history running back half a century — and the right move is to claim lineage, not novelty. [I. J. Good](https://people.eecs.berkeley.edu/~russell/papers/aij-cnt.pdf) called it Type II rationality in 1971: maximize expected utility *with the costs of deliberation included*. [Russell and Wefald](https://www.sciencedirect.com/science/article/abs/pii/000437029190015C) made it mechanical in 1991 as the *value of computation* — each computation is an action, worth its expected improvement in decision quality minus its cost. The metareasoning reward above is that idea reaching LLM training, and [TurKontrol](https://ojs.aaai.org/index.php/AAAI/article/view/7760) even ran the logic over people in 2010: a POMDP deciding whether another improvement pass, another vote, or submission had the highest expected utility. The classical work priced homogeneous internal steps. What's genuinely new is that a "computation" can now be a delegation — a stochastic call to another mind whose context you construct — so the allocator has to carry a theory of capability, not just pick the next node to expand.

None of that ancestry changes what's at stake now. The interesting frontier is not teaching an LLM to call a subagent — mechanically, that's trivial. It's teaching the **economics of intelligence**: when another unit of intelligence is worth purchasing, where to spend it, and when the result is good enough to stop. If that's right, it changes what labs should train for. The capability of a deployed system stops being the capability of its biggest model and becomes the quality of its allocation policy — the manager's judgment, not only the genius's insight. A lab that trains delegation directly, under a reward that prices tokens, latency, and calls, gets to run cheap models productively wherever an expensive one can construct the problem correctly first.

There's a further step down this staircase — the orchestrator compiling tasks so far down the capability–cost spectrum that no intelligence is required at all, the claim that intelligence can transform work into a form requiring less intelligence — but that's the next essay.
