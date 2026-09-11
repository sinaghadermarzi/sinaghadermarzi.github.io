---
title: "How Much Can an Interview Actually Measure?"
draft: true
date: 2026-07-29
description: "Thinking about evaluation resolution and signal quality."
---

Somewhere this week, a 99th-percentile engineer is being rejected by an interview loop that cannot tell her apart from a 90th-percentile one. Nobody in the room is lying and nobody is incompetent; the loop simply ran out of measurement. Above a certain level of ability, every candidate solves the graph problem, communicates clearly, and says sensible things about trade-offs — additional true skill produces no distinguishable observation. The interview has saturated, the way a photodetector saturates in bright light, and the company will nonetheless walk away believing it has learned something precise about her.

I've come to think this is not a story about interviews. It is a story about instruments, and claims of measurement, and it applies with uncomfortable exactness to how we evaluate AI systems today.

## An evaluation is an instrument, and instruments have specs

When a physicist buys a voltmeter, the datasheet doesn't say "good" or "bad." It specifies what the device can and cannot resolve. I want to read evaluations — interviews, benchmarks, judge models, performance reviews — the same way, with five spec-sheet properties:

- **Validity:** does the evaluation measure the intended ability?
- **Reliability:** would repeated or independent evaluations give similar results?
- **Resolution:** what is the smallest ability difference the system can reliably distinguish?
- **Dynamic range:** over what ability interval is the instrument informative rather than floored or saturated?
- **Calibration:** does evaluator confidence correspond to actual accuracy?

The consequence of taking this seriously is that a measured result is never a property of the candidate alone. It is an interaction:

$$
\hat A = \mathcal{E}(X)
$$

The measured ability is what the whole evaluation system — judges, tasks, procedure, organizational context, feedback and calibration history — produces when applied to the person or model being measured. Change the instrument and you change the number, even when the underlying ability holds still.

For interviews, these are measured properties, not metaphors. When [interviewing.io](https://interviewing.io/blog/technical-interview-performance-is-kind-of-arbitrary-heres-the-data) tracked the same engineers across many standardized interviews, [only about a fifth](https://interviewing.io/blog/after-a-lot-more-data-technical-interview-performance-really-is-kind-of-arbitrary) performed consistently, and volatility was essentially unrelated to strength (R² ≈ 0.03): strong engineers randomly bomb. And a [randomized whiteboard experiment](https://news.ncsu.edu/2020/07/tech-job-interviews-anxiety/) found that being watched while solving problems roughly halved performance — the authors' verdict was that the format tests performance anxiety as much as coding skill. A noisy instrument, partly aimed at the wrong construct, that has never measured either fact about itself.

I should say immediately that psychometrics got here first. Item response theory's [test information function](https://www.cogn-iq.org/learn/theory/test-information-function/) already formalizes conditional precision: a test can measure sharply in one ability region and barely at all in another, and where information runs out, the standard error balloons. And for AI specifically, ["Metrology for AI: From Benchmarks to Instruments" (Welty, Paritosh & Aroyo, 2019)](https://arxiv.org/abs/1911.01875) argued years ago that evaluation *is* measurement and that instruments are characterized by precision and by resolution — the smallest detectable change. Forecast verification owns part of my vocabulary too: in [Murphy's 1973 decomposition](https://journals.ametsoc.org/view/journals/apme/12/4/1520-0450_1973_012_0595_anvpot_2_0_co_2.xml) of the Brier score, *resolution* already names whether a forecaster's judgments sort cases into genuinely different outcome classes — a cousin of my instrument sense, not the same quantity, but close enough that I'm borrowing the word rather than coining it. What I'm adding is mostly a transfer and a norm: the full instrument spec applied to informal, never-calibrated evaluations like technical interviews, plus an explicit rule about what conclusions such instruments license.

## Taste runs ahead of skill

Here is the property of evaluators that makes any of this workable: productive competence and evaluative competence are not the same thing. A person can recognize performance beyond what they can personally produce. Ira Glass's famous ["taste gap"](https://www.themarginalian.org/2014/01/29/ira-glass-success-daniel-sax/) is the craft version — for years your taste outruns your output, and your taste is what tells you your output isn't good yet. The LLM literature has now measured the same dissociation directly: in the [generation–verification-gap work](https://arxiv.org/html/2606.28050), a model with roughly 15% generation accuracy on a task reached 85% accuracy as a judge of others' solutions. Recognition and production come apart, in both machines and people. (The [Dunning–Kruger line](https://doi.org/10.1037/0022-3514.77.6.1121) is the standing counterpoint — in some domains evaluative skill leans on the very skill being evaluated — which is exactly why the two competences deserve to be treated as separate axes rather than assumed equal.)

This matters because it locates the ceiling of an evaluation correctly. The relevant limit is not "can your interviewers do the job better than the candidate" but their *discriminative* competence: domain model, reference examples, evaluation experience, feedback, and how informative the tasks are. An organization can, in principle, evaluate above its own productive level. It just cannot evaluate above its discriminative level — and most organizations have never measured either.

## The Evaluative Authority Principle

Once you see evaluations as instruments with finite specs, one norm follows, and I think it deserves a name. Call it the **Evaluative Authority Principle**:

> The strength of an evaluative conclusion must not exceed the demonstrated measurement capability of the system producing it.

Equivalently:

$$
\text{strength of claim} \leq \text{demonstrated strength of instrument}
$$

In words: you may conclude from an evaluation only as much as you can show the evaluation could actually measure. A four-hour loop with uncalibrated interviewers and no feedback history has some demonstrated capability — probably "can distinguish clearly-unqualified from plausibly-qualified" — and conclusions should be sized to that, not to the ceremony surrounding the decision.

Nor is this a special insult to interviews. When NeurIPS [ran a tenth of its submissions past two independent review committees](https://ar5iv.labs.arxiv.org/html/2109.09774), the committees disagreed on roughly a quarter of papers, about half the accepted list would have changed on a rerun, and the revisit's verdict was a process good at identifying poor papers and poor at identifying good ones; the [2021 replication](https://arxiv.org/abs/2306.03262) got essentially the same numbers. Demonstrated capability: rejecting the clearly weak. Every acceptance claimed more.

The principle forces a distinction I find clarifying: decision authority is not measurement authority. A company is entitled to choose whom it hires; that entitlement is absolute. But rejection does not automatically constitute objective evidence that the rejected candidate lacks the claimed ability. The decision is legitimate; the *measurement claim* implied by the decision usually isn't. Most of the psychological damage interviews inflict comes from smuggling the second out of the first.

The norm itself has a seventy-year empirical ancestry, and I would rather name it than appear to discover it. The clinical-versus-actuarial tradition — [Meehl in 1954](https://doi.org/10.1037/11281-000), the [*Science* summary](https://doi.org/10.1126/science.2648573) by Dawes, Faust and Meehl, a [meta-analysis of 136 studies](https://doi.org/10.1037/1040-3590.12.1.19) — spent decades checking expert institutional judgments about people (prognosis, parole, personnel selection) against simple mechanical formulas, and the formulas equaled or beat the experts in the vast majority of comparisons. [Grove and Meehl](https://doi.org/10.1037/1076-8971.2.2.293) drew the moral as an ethics claim: keeping informal judgment where a formula demonstrably does better is unearned authority. Institutions mostly ignored the result for decades — which is rather the point.

## The Evaluative Efficiency Principle

Instruments cost money to run, and here the framework needs a second axis. A crude version is information per unit cost:

$$
\eta(\mathcal E) = \frac{I(A;X)}{C(\mathcal E)}
$$

— how much the evaluation's observations actually tell you about the ability, divided by what the evaluation costs to run. But the crude version misleads, because the question about interview round five is never "does it contain any information?" (it always contains some); it is whether its *marginal* decision value exceeds its *marginal* total cost — where the cost ledger honestly includes organizational time, candidate time and preparation, delay, false positives and negatives, candidate withdrawal, and the legitimacy cost of a process that feels arbitrary to the people inside it. Hence the **Evaluative Efficiency Principle**:

> An evaluation is justified only when the expected decision-relevant information it adds exceeds the full cost of obtaining it.

This is a stopping rule, and it has a distinguished lineage. [Wald's sequential probability ratio test](https://egyankosh.ac.in/bitstream/123456789/112630/1/Unit-6.pdf) (1945) is the canonical stop-when-evidence-suffices result. [Howard's information value theory](https://www.semanticscholar.org/paper/Information-Value-Theory-Howard/a7b3c2a88ca459d50010a33db8c2f113f1323e0c) (1966) priced information for decision makers. [Cronbach and Gleser (1957)](https://books.google.com/books/about/Psychological_Tests_and_Personnel_Decisi.html?id=K_GyAAAAIAAJ) treated personnel testing as an aid to decisions rather than measurement for its own sake — marginal value versus cost, for hiring, seventy years ago. Computerized adaptive testing runs [stopping rules](https://pmc.ncbi.nlm.nih.gov/articles/PMC3028267/) that end a test when predicted information gain gets small. And Google's ["Rule of Four"](https://rework.withgoogle.com/blog/google-rule-of-four/) found it empirically: four interviewers predicted the final hiring outcome with 86% confidence, each additional interviewer added less than 1% predictive accuracy, and cutting the loop saved about two weeks of time-to-hire. Marginal information collapses; marginal cost doesn't. What the principle adds to this lineage is the widened ledger — candidate time, withdrawal, and legitimacy are real costs that classic value-of-information analyses never priced — and the coupling to resolution: stop not just when information is expensive, but when the instrument has no resolution left where the remaining candidates live.

## Benchmarks are interviews for machines

Everything above lands on AI evaluation with almost no translation required, because the benchmark lifecycle *is* instrument saturation replayed in public, on an eighteen-month clock.

SWE-bench Verified is the cleanest case. It climbed from roughly 33% in mid-2024 to about 81% by late 2025, then flattened — Claude Opus 4.6 essentially tied Opus 4.5 on it while harder benchmarks kept moving. Audits found that at least 59.4% of the examined still-unsolved problems had flawed tests rejecting functionally correct patches, that models could identify the buggy file from the issue text alone far above chance, and that the same model generation scoring ~81% on Verified scored ~23% on its decontaminated successor. In February 2026 OpenAI publicly stopped reporting the benchmark. I read that retirement as the Evaluative Authority Principle enforced in the open: the demonstrated capability of the instrument no longer supported the claims being made on it, so the claims had to stop. One practitioner essay put the saturation point perfectly: [ranking models by a saturated benchmark is ranking them by the residual error of the benchmark](https://benchmarkingagents.com/what-these-benchmarks-miss/) — when frontier models [cluster within a few points](https://www.digitalapplied.com/blog/llm-benchmark-methodology-2026-contamination-leaderboard-guide) of each other near the ceiling, the differences are the instrument's noise floor, not capability.

The other instrument properties have AI-shaped failures too. Reliability without validity: a [large-scale evaluation of LLM judges](https://arxiv.org/abs/2606.19544) found test–retest reliability above 0.95 coexisting with severe position bias — a perfectly consistent instrument measuring the wrong thing, which is why consistency licenses nothing (the judge-bias literature is now a subfield). Dynamic range: pass^k exists because pass@1 saturates exactly where deployment questions begin — an agent at ~61% single-attempt success collapses below 25% at eight consecutive attempts, and a 90% pass@1 agent completes eight runs in a row only ~43% of the time. Reliability is a different region of the ability axis, and it needed a different instrument. And calibration of the whole apparatus: the LoCoMo forensics in the memory literature found a ten-conversation benchmark serving as the field's leaderboard with 6.4% of its answer key wrong, an LLM judge that accepted 62.81% of intentionally wrong answers, 56% of per-category system comparisons statistically indistinguishable from noise — and one vendor's score moving from 84% to 58% to 75% depending on who ran the harness. That is not a scoreboard. That is an uncharacterized instrument being read to three significant figures.

And the failure now flows the other way too: interviews are failing like benchmarks. When interviewing.io [planted candidates covertly using ChatGPT](https://interviewing.io/blog/how-hard-is-it-to-cheat-with-chatgpt-in-technical-interviews), they passed 73% of interviews that used verbatim LeetCode questions, and no interviewer detected it; by 2025, [81% of surveyed Big Tech interviewers](https://interviewing.io/blog/how-is-ai-changing-interview-processes-not-much-and-a-whole-lot) had suspected AI use in a remote interview but only 31% had ever confirmed a catch. The instrument's validity domain shifted underneath it — contamination, by another name — and re-zeroing costs real money: [in-person interviews went from roughly 5% of roles in 2024 to roughly 30% in 2025](https://www.computerworld.com/article/4044734/to-counter-ai-cheating-companies-bring-back-in-person-job-interviews.html). The lifecycle is the same because the instrument problem is the same.

## Measuring the measurers

If evaluations are instruments, the claims organizations make about their own evaluative needs become measurable — and job postings are the public record of those claims. The empirical program: construct a latent **claimed evaluation difficulty**, $D_{claim}$, from a posting's seniority, breadth and rarity of required skills, years demanded, and prestige language ("exceptional," "world class," "deep expertise"). Separately estimate the organization's plausible **evaluative capacity**, $C_{eval}$, from external evidence — incumbent-team expertise, technical accomplishments, publications, open-source work, product complexity. Then define the **evaluative authority gap**:

$$
G = D_{claim} - C_{eval}
$$

and test whether larger gaps predict prolonged vacancies, repeated postings, requirement retraction, higher interview burden, and turnover, controlling for compensation, location, size, scarcity, and the macro cycle. A crisp opening question: do organizations demand more candidate capability than their observable technical population demonstrates, and does that discrepancy predict difficulty filling positions? A companion measure compares nominal level distinctions (Senior, Staff, Principal) against demonstrated differences among incumbents — **nominal versus effective evaluative resolution**.

What convinces me this is feasible is that nearly every piece of the machinery already exists. The credential-gap literature measured postings against incumbent attainment: [67% of production-supervisor postings demanded a degree only 16% of incumbents held](https://www.hbs.edu/managing-the-future-of-work/Documents/dismissed-by-degrees.pdf), and ["Moving the Goalposts"](https://www.luminafoundation.org/files/resources/moving-the-goalposts.pdf) found credentials gaps up to ~25 points in middle-skill roles. The [upskilling research](https://direct.mit.edu/rest/article/102/4/793/96774/Upskilling-Do-Employers-Demand-Greater-Skill-When) showed posted requirements rise and fall with labor-market slack — 18–25% of the 2007–2010 requirement increases were opportunistic, and were retracted as markets tightened — so requirements are strategic artifacts, not descriptions of jobs. The [ghost-jobs literature](https://www.columbialawreview.org/content/ghost-jobs/) shows around a fifth of postings don't correspond to genuine openings at all, with hires per posting roughly halved from 2019 to 2024. And [vacancy-duration measurement](https://fred.stlouisfed.org/series/DHIDFHDMIR) has existed as standardized machinery since 2013. Every dependent variable is measured; every adjacent gap has been quantified. The one thing missing is the regressor itself. The closest tradition — the clinical-versus-actuarial audits above — checked evaluators retrospectively, against outcomes that eventually arrived; I have found no study that operationalizes an organization's *capacity to evaluate what it demands* prospectively, from its public exhaust. That gap is the opportunity.

The instrument view won't tell you whom to hire or which model to ship. It tells you something quieter and, I think, more durable: before you trust a number, ask what the device that produced it has demonstrably resolved — and stop paying for measurements the instrument can no longer make.
