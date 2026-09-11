---
title: "Typology as Clue, Not Blueprint"
# draft: true
date: 2026-08-23
description: "Personality typologies fail as science, but they preserve a question worth stealing for artificial minds: do systems differ systematically in how they metabolize information?"
---

An AI engineer citing MBTI is a little like a chemist citing alchemy: whatever point you are making, the reader has decided something about you before the paragraph ends. So let me make the concession first, in full, before explaining why I am citing this material anyway. As psychology, personality typologies are close to worthless, and I will report exactly how worthless below. What they preserve is not an answer but a question — and it happens to be a question I need, because this series argues that agents built on identical base models may grow into structurally different cognitive architectures. That claim has an obvious family resemblance to "people come in cognitive types," and I would rather examine the family history than hope nobody notices it.

## The record, plainly

Start with the least controversial part. The MBTI's core empirical claim — that people fall into sixteen discrete types — has failed on its own terms for decades. In [Pittenger's 1993 review](https://eric.ed.gov/?id=EJ475507), with a test–retest interval as short as about five weeks, as many as 50% of takers were reclassified into a different type. A measurement instrument whose categories flip on a coin toss inside five weeks is not measuring categories.

The deeper failure is statistical. If discrete types were real, they would leave a signature in the data:

$$
p(\theta) \;=\; \sum_{k}\pi_k\, f_k(\theta), \qquad \text{with well-separated modes}
$$

In words: measured scores should look like a mixture of distinct clusters — bimodal or multimodal humps — not one centered hump. Early MBTI literature did report bimodal distributions. Then [Bess and Harvey (2002)](https://www.tandfonline.com/doi/abs/10.1207/S15327752JPA7801_11) re-scored roughly 12,000 people with item response theory and found the bimodality had been an artifact of the scoring software's default settings; properly scored, the distributions are strongly center-weighted. The statistical evidence for human types was, in a very literal sense, a software bug. Mainstream personality science models people as points in continuous trait space, and [Stein and Swan (2019)](https://compass.onlinelibrary.wiley.com/doi/abs/10.1111/spc3.12434) treat MBTI's persistence mostly as a window into our intuitive appetite for categories.

[Socionics](https://en.wikipedia.org/wiki/Socionics) — the Soviet-era cousin — fares worse. It was developed in 1970s–80s Lithuania by [Aušra Augustinavičiūtė](https://en.wikipedia.org/wiki/Au%C5%A1ra_Augustinavi%C4%8Di%C5%ABt%C4%97), an economist by training, who fused Jung's typology with the psychiatrist [Antoni Kępiński's](https://en.wikipedia.org/wiki/Antoni_K%C4%99pi%C5%84ski) concept of "information metabolism" into sixteen types plus an elaborate theory of relations between them. It has essentially no validation literature that meets mainstream psychometric standards; the one peer-reviewed English academic treatment I can find, [Pietrak (2017)](https://www.sciencedirect.com/science/article/abs/pii/S1389041717301365), is expository rather than validating, and the standard English-reference characterization is blunt: pseudoscientific. Even sober cognitive-style research keeps dissolving its own typologies under measurement — [Blazhenkova and Kozhevnikov (2009)](https://onlinelibrary.wiley.com/doi/10.1002/acp.1473) had to abandon the classic bipolar visualizer–verbalizer dimension when confirmatory factor analysis showed a three-factor model fits better. The pattern is consistent: put a human typology under a proper instrument and it melts into continuous traits.

## The question under the wreckage

So why bring any of this up? Because underneath the unvalidated answers sit two unusually good questions.

[Jung's 1921 functions](https://psychclassics.yorku.ca/Jung/types.htm), read charitably, are not claims about what people value but about how experience gets *selected and transformed* — proto-hypotheses about information processing, backed by clinical anecdote rather than measurement. Kępiński's information metabolism made the frame explicit: the psyche as a system that ingests, transforms, and exchanges information with its environment, on analogy with energy metabolism in a cell. And Augustinavičiūtė's genuinely unusual move was the second question: she tried to theorize the *relations* between information-processing types — which pairings are productive, which grind. Scientific psychology largely never asked that question at all.

Strip the sixteen types away and what remains is:

1. Do minds differ systematically in how they select, transform, and exchange information?
2. If they do, which couplings between different information-processors are complementary?

Both questions transfer to artificial systems cleanly — better than they ever applied to humans, I will argue. The first is the phenotype question this series keeps circling: whether persisting essays versus scripts, nodes versus links, archives versus living topics produces different kinds of minds. The second becomes measurable for machines: how much does an architecture gain from consuming another architecture's artifacts? That is the compatibility matrix of [Artifact Compatibility](/thoughts/artifact-compatibility/) — intertype relations rebuilt as an experiment instead of a doctrine.

## Types versus traits, head-on

Here is the objection I cannot dodge: if human "types" dissolved into continuous traits the moment someone measured properly, why would artificial types be any more real? Isn't this series repeating psychology's mistake with worse instruments?

The honest answer is that artificial types, if they exist, will be real for a different reason: **discreteness by engineering**. Human cognitive variation is biological, and biology handed us continuous distributions. Architectural variation is chosen, and choices are categorical. A persistent store either is a code library or it is not. A knowledge base either has a refresh process or it does not. Links are typed or untyped; provenance is recorded or discarded. When I configure an agent to persist essays rather than scripts, I place it in a discrete cell of a design matrix, and the cell boundary is enforced by the file format — not discovered, precariously, in a score distribution. Types failed as a *description* of humans; they might work as a *specification* for machines, because specifications are exactly the kind of thing that is allowed to be discrete.

Two caveats keep this from becoming its own astrology. First, engineered discreteness guarantees the categories exist, not that they matter — the cells of the matrix could all behave identically. Whether discrete architectural choices produce measurably different minds is an empirical question about antagonistic objectives, which is the subject of [Tradeoffs Make Minds](/thoughts/tradeoffs-make-minds/). Second, labels alone demonstrably do nothing: across [162 personas, four model families, and 2,410 questions](https://arxiv.org/abs/2311.10054), telling a model what kind of thinker it is produced no gains and sometimes slight losses. If artificial cognitive types are real, they will be made of accumulated persistent structure, not a line of prompt text.

## Clue, not blueprint

Which leaves the question of order of operations. The wrong program starts from a human typology and builds agents in its image — sixteen prompt personalities, "dual" pairings, the works. That inherits every unvalidated assumption and adds new ones. The right program runs the other way:

1. **Discover the computational tradeoffs experimentally.** Which objectives are antagonistic enough that one architecture cannot serve both? Biology offers at least one existence proof in [complementary learning systems](https://www.cell.com/trends/cognitive-sciences/abstract/S1364-6613(16)30043-2), but the artifact-level tradeoffs must be measured, not assumed.
2. **Identify the stable artificial architectures.** Which configurations persist and compound under long-horizon operation, rather than eroding into each other?
3. **Study complementarity.** Measure the couplings between architectures directly; do not theorize duality in advance.
4. **Only then look back.** Ask whether anything in the measured structure resembles the old human typologies — and treat any resemblance as a curiosity to explain, not a validation of the source.

Typology enters at step four, as a comparison class, and before that only as a vocabulary and a hunch generator. That is what "clue" means: Augustinavičiūtė gets credit for asking about couplings fifty years early, and no authority over the answer. The experimental program that makes steps one through three concrete is laid out in [Growing Minds in the Lab](/thoughts/growing-minds-in-the-lab/).

**What would change my mind:** A preregistered, replicated study validating socionics' intertype predictions in humans — blinded pairs classified as complementary actually collaborating measurably better — would promote typology from clue to design prior, and I would import its structure directly. In the opposite direction, if long-lived agent deployments turn out to spread continuously across architecture space with no stable clusters, then "type" is the wrong noun for machines too, and this series should speak only of axes and traits.
