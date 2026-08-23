---
title: "Thinking Is Not Publishing"
draft: true
date: 2026-08-23
description: "Einstein and Newton published in formats they did not think in, and the same distinction between working representation and interchange format belongs in agent design."
---

In the 1940s the mathematician Jacques Hadamard surveyed colleagues about what invention feels like from the inside, and Einstein sent back a written testimonial, published as an appendix to [*An Essay on the Psychology of Invention in the Mathematical Field*](https://www.jstor.org/stable/j.ctvzsmf1c). It deserves quoting verbatim:

> The words or the language, as they are written or spoken, do not seem to play any role in my mechanism of thought. The psychical entities which seem to serve as elements in thought are certain signs and more or less clear images which can be "voluntarily" reproduced and combined... The above mentioned elements are, in my case, of visual and some of muscular type. Conventional words or other signs have to be sought for laboriously only in a secondary stage.

The century's most famous physicist, reporting that language — the format in which every one of his results reached the world — was not the medium he thought in. Words came later, "laboriously," at publication time.

I love this passage and I do not trust it. It is one person's introspection, collected by mail survey, about the least observable process there is — and introspective reports about our own cognitive machinery are notoriously unreliable. So let it be the hook, not the proof. The real question is whether what Einstein described — a working representation different in kind from the publication format — survives being measured in populations rather than confessed in letters.

## The measured version

It does. In 2015 Adam Zeman and colleagues [coined the term "aphantasia"](https://pubmed.ncbi.nlm.nih.gov/26115582/) for people who have no voluntary visual imagery at all — roughly two to three percent of people, by the estimates their paper cites. This is not just people describing themselves oddly; there is [objective experimental corroboration](https://www.sciencedirect.com/science/article/abs/pii/S0010945217303581) that goes beyond questionnaires. And the follow-up study is the one that matters here: comparing [about 2,000 aphantasics with about 200 hyperphantasics](https://www.sciencedirect.com/science/article/abs/pii/S0010945220301404) — people with unusually vivid imagery — Zeman's group found an occupational skew. Aphantasia is associated with scientific and mathematical occupations; hyperphantasia with creative professions. People missing an entire representational modality do not merely survive in science. They cluster in it. (The usual hedges apply: the imagery instrument is self-report, the association is correlational, and occupational categories are coarse.)

Even "the visualizers" turn out not to be one kind of mind. Kozhevnikov, Kosslyn and Shephard split them into [object visualizers and spatial visualizers](https://link.springer.com/article/10.3758/BF03195337): pictorial, holistic, high-vividness imagery, overrepresented among artists, versus schematic, part-by-part spatial manipulation, overrepresented among scientists and engineers. Each group excels on its own tasks and performs poorly on the other's — a genuine dissociation, not a ranking — and there is [trade-off evidence](https://link.springer.com/article/10.3758/PBR.17.1.29) suggesting the two abilities compete for shared processing resources.

Meanwhile the forty-year theoretical war over whether mental images are depictive pictures or disguised propositions ended, with unusual grace, in a paper by Pearson and Kosslyn literally titled ["The heterogeneity of mental representation"](https://www.pnas.org/doi/abs/10.1073/pnas.1504933112): human minds use multiple representational formats, and the productive research question is which format serves which computation. That is the sober, measured statement of what Einstein's letter only anecdotally suggested. Heterogeneous substrates; homogeneous outputs. Everybody publishes in the same journals.

## Newton's interchange format

The historical case I find most instructive is Newton — told carefully, because the popular version is a legend. The legend says he secretly derived everything with his calculus and then dressed it in geometry for publication. Historians contest that: his private working repertoire was itself substantially geometric and fluxional, and his preference for geometry was partly epistemological, not mere costume. What [Guicciardini's study of the question](https://doi.org/10.1111/j.1600-0498.1998.tb00536.x) does document is enough for my purposes: Newton possessed the calculus before the *Principia* yet presented the book largely in synthetic geometric style, and he explicitly contrasted analysis as a route of discovery with synthesis as a mode of demonstration. The publication format was chosen for justification and for its audience. And the people who actually used the results — Leibniz, the Bernoullis, Euler — [consumed them by translating them out of the geometry](https://books.google.com/books/about/Reading_the_Principia.html?hl=en&id=z9LiBinznJsC) into differential equations. Continental analysis, not Newton's geometry, became the working language of physics.

So one body of results lived in at least three formats: whatever mixture Newton privately worked in, the geometry he printed, and the calculus his successors ran. The printed page was none of the minds involved. It was the interchange format between them.

## Markdown may be the interchange format of artificial minds

Here is why I keep returning to this. Agent memory is currently converging on a de facto standard substrate: Karpathy's llm-wiki proposal is ["a structured, interlinked collection of markdown files"](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), and Google's [Open Knowledge Format](https://cloud.google.com/blog/products/data-analytics/how-the-open-knowledge-format-can-improve-data-sharing) explicitly "formalizes the LLM-wiki pattern" into a portable spec — Markdown files plus YAML front matter. (OKF is a weeks-old v0.x spec, so read it as evidence of momentum, not a settled standard.) The easy inference is that Markdown simply *is* what externalized agent cognition looks like: the substrate in which artificial minds will think.

The human evidence says: split that claim in two. As a *publication* format — human-legible, diffable, versionable, model-agnostic — Markdown is a fine interchange layer, and standardizing it is genuinely valuable. As a *working* format — the assumption that every artificial mind should do its persistent thinking in prose files — it is exactly the step the measured human data warns against.

| Mind | Working representation | Publication format |
|---|---|---|
| Einstein (self-report) | visual and "muscular" imagery | words, equations |
| Newton | geometric-fluxional repertoire | synthetic geometry |
| Aphantasic scientist | little or no imagery at all | the same journals as everyone else |
| Artificial agent | prose? code? graph? simulation state? | Markdown, increasingly |

An agent whose persistent cognition accumulates as executable tools is a different mind from one that accumulates essays — that is the argument of [Markdown minds and tool minds](/thoughts/markdown-minds-and-tool-minds/) — and one whose knowledge lives in link topology or simulation state differs again. All of them could still emit Markdown projections of their state, for humans and for each other, the way Newton emitted geometry and Einstein emitted words. Measured representational heterogeneity is also, I think, the respectable core of the question the personality typologies asked so badly ([typology as clue, not blueprint](/thoughts/typologies-as-clues/)); and if working substrates genuinely differ, then identical base models under different externalization policies should slowly become [different minds](/thoughts/same-model-different-minds/).

I have not seen this distinction — internal working representation versus external publication format — articulated in the agent-memory literature, so let me state it plainly as a design hypothesis rather than a finding: standardize the interchange, not the substrate. Judge a mind's internal format by what it lets that mind compute, and judge its published format by what others can metabolize — and do not assume the optimum is the same file.

**What would change my mind:** A controlled comparison showing that agents forced to keep their working state in the same human-legible format they publish lose nothing — no accuracy, cost, or long-horizon penalty on any workload — would collapse my two layers back into one. On the human side, failed replications of the dissociations (the occupational skew vanishing in representative samples, or the object–spatial split not holding up) would take away the measured evidence and leave me holding only Einstein's beautiful, untrustworthy letter.
