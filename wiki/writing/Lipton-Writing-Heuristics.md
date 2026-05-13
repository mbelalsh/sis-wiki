---
title: Lipton — Heuristics for Scientific Writing (a Machine Learning Perspective)
tags: [writing, heuristics, ml-perspective, abstract, organization, style, language, citation]
sources:
  - raw/writing/guides/Heuristics for Scientific Writing (a Machine Learning Perspective) – Approximately Correct.pdf
created: 2026-05-13
updated: 2026-05-13
sis_relevance: high
hard_constraint_possible: no
---

# Lipton — Heuristics for Scientific Writing (a Machine Learning Perspective)

**Author:** Zachary C. Lipton, Carnegie Mellon University.
**Source:** Approximately Correct blog, January 29, 2018.
**Format:** 17-page blog post, 17 named heuristics across 5 sections + 21
comment threads.
**Origin lineage:** "I learned early in my PhD from Charles Elkan that many
important heuristics for scientific paper writing can be summed up in
snappy maxims."

## Origin Statement (Verbatim)

> "The following list consists of easy-to-memorize dictates, each with a
> short explanation. Some address language, some address positioning, and
> others address aesthetics. **Most are just heuristics so take each with
> a grain of salt, especially when they come into conflict. But if you're
> going to violate one of them, have a good reason.**"

## Why This Heuristic List Matters

Lipton frames the post as a counter-pressure to ML's growth: many
submitted papers are unreadable; poor writing damns some to rejection and
others to later test-of-time criticism. The list is a defense kit against
the most common ML-paper writing failures.

> "Of the thousands of papers that hit the arXiv in the coming month, many
> will be unreadable. Poor writing will damn some to rejection while
> others will fail to reach their potential impact."

## The 17 Heuristics (Section-by-Section, Verbatim Names)

### Section 1: The Introduction (5 heuristics)

**1. KEEP YOUR ABSTRACT SHORT.**

> "You can't get it all out in the abstract. Don't even try. **Think of the
> abstract as the 2-minute spotlight talk advertising your paper.**"

Lipton's tried-and-true 4-bullet abstract formula:

1. Contextualize the problem in either one sentence or one phrase
2. Identify what's wrong with existing approaches
3. Go big: clearly state your major contribution (can also lead with this)
4. Two or three sentences to sell the details, major quantitative result, etc.

**Exemplar (Sanjoy Dasgupta, "Learning Mixtures of Gaussians"):**

> "Mixtures of Gaussians are among the most fundamental and widely used
> statistical models. Current techniques for learning such mixtures from
> data are local search heuristics with weak performance guarantees. We
> present the first provably correct algorithm for learning a mixture of
> Gaussians. The algorithm is very simple and returns the true centers of
> the Gaussians to within the precision specified by the user, with high
> probability. It runs in time only linear in the dimension of the data
> and polynomial in the number of Gaussians."

**2. DON'T TEASE THE READER.**

> "If you have a great quantitative result, stick the number right in the
> abstract and the introduction. If your paper yields a single equation
> that can be operationalized, stick it right in the introduction. **People
> should read on because they are interested, not because you are teasing
> them by withholding information.**"

**3. DELETE GENERIC OPENINGS.**

Examples to delete: "The last 10 years have witnessed tremendous growth in
data and computers." / "Deep learning has had many successes at many
things."

> "**If the first sentence for your paper can be pre-pended to any paper in
> all of ML/big data, delete it.** First impressions matter. The first
> sentence is the most precious real estate in your introduction. Don't
> squander it."

**4. Q BEFORE A.**

> "It's difficult to get excited about a solution if you don't believe
> there is a problem. [...] If possible: lead with a compelling real-world
> example, formalize it as an abstract problem, and then close the loop
> with experiments that address the motivating case."

**5. FOCUS ON WHAT YOUR METHOD DOES, NOT WHAT IT DOESN'T DO.**

> "When all else is equal (semantically), it's much more readable to ditch
> the indirection and just say precisely what something is, not what it
> isn't. This is especially true for your own methods."

### Section 2: Organization (4 heuristics)

**6. WORDS ARE NOT SENTENCES. SENTENCES ARE NOT PARAGRAPHS. PARAGRAPHS ARE
NOT SUBSECTIONS. SECTIONS CONTAIN MORE THAN ONE (OR ZERO) SUBSECTIONS.
PAPERS CONTAIN MORE THAN ONE SECTION.**

> "One immediate tell that you are engaging with a lousy writer is that the
> paper doesn't look right, before you read a single word. Sections, like
> bullets on slides should be balanced. **But the safe heuristic is
> paragraphs have 3 sentences minimum.**"

**7. A READER SHOULD UNDERSTAND YOUR PAPER JUST FROM LOOKING AT THE
FIGURES, OR WITHOUT LOOKING AT THE FIGURES.**

> "A blind reader should understand precisely what you do, even if they
> miss a couple granular bits of data captured in figures. **Any critical
> observation or technical details must appear in the paper's main text,
> which can reference figures for visual corroboration.**"
>
> "Similarly, the figures should tell a coherent story. If your reader
> skips to the figures (**reviewers will**), they should be able to see
> roughly what's going on and understand the significance of the findings.
> If it's not obvious whether higher or lower scores on y axis are better,
> the caption ought to say this."
>
> "A good caption should be between 1 and 3 lines."

**8. QUICKLY ARRIVE AT THE PAPER'S CONTRIBUTION.**

> "Longwinded front-matter in conference papers (less applicable to journal)
> is bad for the following reasons: (1) Reviewers read 5-10 papers per
> conference and 50-100 papers per year in very similar areas. The basics
> will bore them. (2) **If your contribution starts on page 5/8, you have
> very little excuse for having failed to do anything the reviewer asks
> for.**"

**9. ANTICIPATE THE READER'S QUESTIONS AND ANSWER THEM IN THE PAPER.**

> "A good reviewer will try to come up with critical questions to challenge
> the proposed work. **Is it possible that this method only works because
> X?** If the answer is 'I don't know' and 'no' would be damning, your
> paper might rightly be rejected. **If you can anticipate the question
> and know the answer, write it. If you do not know the answer, then run
> an experiment to find out.** I hope this point hits home that doing
> strong research and writing clearly are tightly linked."

### Section 3: Style (4 heuristics)

**10. THE SCIENTIFIC "WE".**

> "In scientific writing, narrate with the pronoun 'we'. This style serves
> a didactic purpose: **'we' refers to 'you' (the reader) and 'I/we' (the
> authors) together.**"

(Edit-update at bottom: "the only agents of interest to science papers
should be other papers. 'We the authors' — means 'We the authors of *this
paper*' not we, real, people who happened to author this paper. **This is
extremely important, it's what gives us the power to decide in the future
to disagree with our present arguments and to find flaws in our present
methods.**")

**11. AVOID HOSTAGES TO FORTUNE.**

> "Any qualified reader, who goes through your entire draft, even if they
> do not share your opinions, preference for methods, or values in life,
> should be unable to disagree with any sentence in isolation. **'Our
> method X outperforms Y on most datasets.' Does it? Most out of what
> collection of datasets?** Could your reviewer choose some dataset
> repository and find the statement false? Better to say 'many' datasets.
> This is both better defined and much harder to disagree with."

**12. A SIN OF OMISSION IS BETTER THAN A SIN OF COMMISSION.**

> "Related to the above: if you are not 100% sure about a claim, do not
> make it. **It's hard to imagine the reviewers rejecting a paper because
> you omitted a one-line boast. It's easy to imagine one line inspiring a
> rejection.**"

**13. WHEN YOU MUST EXPRESS AN OPINION, IDENTIFY IT AS SUCH.**

> "You can include an opinion, e.g., the great promise of GANs for anomaly
> detection, but **the factual assertion should be that it is your
> opinion**: 'in our opinion, GANs...'."

### Section 4: Language (3 heuristics)

**14. BREAK UP LONG SENTENCES.**

> "Young writers often believe, mistakenly, that long sentences reflect
> language skills. **Great scientific writers write mostly in short
> sentences.** If you find yourself struggling to pack an idea in one
> sentence, it probably requires more than one. Technical writing should
> be as clear as possible. If simplicity is possible, then make the
> writing simple. **The contribution of your paper should be sophisticated
> ideas, not sophisticated sentence structure.**"

**15. JETTISON INTENSIFIERS AND VACUOUS ADVERBS.**

> "Examples: Extremely, Very, Incredibly, Completely, Barely, Essentially,
> Rather, Quite, Definitely, …
>
> Intensifiers are bad for two reasons: (i) they undermine their own
> purpose: 'algorithm X provides a tight approximation' sounds confident,
> while 'algorithm X provides a *very* tight approximation' drips with
> insecurity, and (ii) they express opinions."

(Tom Dietterich adds in comments: avoid "real" and "truly" as in "real
intelligence" or "truly learns abstractions". Daniel Roy adds: "rich" is a
mind virus. Greg Ver Steeg adds: "complex" is vacuous.)

**16. SUBJECTS, VERBS AND MODIFIERS, SHOULD ALL AGREE.**

> "One common mistake in writing is to attribute verbs and modifiers to
> the wrong subjects, e.g. **the algorithm tries to X, or the data is
> biased. Algorithms don't try, just as they don't think.** If we are
> speaking to desires or to intentions, then they belong to 'we', the
> modelers, not the algorithm."
>
> "Corollary: every action should be attributed. **Verbs with no subject
> can often emerge in passive constructions (where the main verb is 'to
> be')**. For example, 'LSTMs are claimed to X, Y, Z'. Who is doing the
> claiming? This information better appear somewhere."

### Section 5: Bibliography (3 heuristics)

**17. CITE GENEROUSLY.**

> "The papers you ought to cite are likely written by the people who will
> be reviewing your paper. [...] If the works are not relevant, then do
> not cite. If they are relevant, you have nothing to lose and much to
> gain by citing.
>
> Among the good karma you'll earn: (1) **you are less likely to get a
> shirty review**, and (2) these are often people you want to work with
> later and **they may notice the citation and read your paper**."

**18. CITE THROUGHOUT.**

> "Reviewers are lazy, and do not have photographic memories. If your work
> builds on others' contributions, do not confine your citations to the
> *related work* section — that's just to summarize your work's context in
> the literature. **Cite throughout the text whenever you invoke methods
> that precede your own.** This is especially true for recent work (last
> 5-10 years), which may not yet be common knowledge and thus confined to
> citation-dense paragraph in the *related work* section."

**19. EXHAUST THE REFERENCES LIMIT.**

> "This is a pragmatic positioning point and applies to conference
> publications that limit the number of pages for references (often 1 or
> 2). If you omit the most related work, reviewers will nail you no matter
> what. But if you omit some borderline related work and they call you on
> it, **having no room left in the references section is a good excuse.
> If you are squatting on a blank bibliography page, don't expect sympathy
> from reviewers.**"

## Comment-Section Extensions (Heuristics Added by Other Researchers)

- **Tom Dietterich (CMU):** avoid "real" / "truly" intensifiers ("real
  intelligence", "truly learns abstractions"). These leave the
  intensified concept undefined.
- **Daniel Roy:** avoid "rich" as an adjective. "A mind virus that stops
  people from actually explaining what is interesting/new/non-trivial."
- **Greg Ver Steeg (USC):** avoid "complex" as vacuous (nobody thinks
  their work is simple); recommends Pinker's *Sense of Style* for the
  "show, don't tell" implementation.
- **Lipton (later edit):** "The only agents of interest to science
  papers should be other papers. 'We the authors' — means 'We the authors
  of *this paper*' not we, real, people who happened to author this
  paper. This is extremely important, it's what gives us the power to
  decide in the future to disagree with our present arguments and to find
  flaws in our present methods."
- **Martin Becker:** OK to add a single impact sentence to a short
  abstract; Lipton concurs ("keep it to the elevator pitch").

## Core Actionable Rules (Compressed)

1. **Use the 4-bullet abstract formula** (context / what's wrong / big
   contribution / quantitative result).
2. **Put the headline number / equation in the abstract AND intro.** No
   teasing.
3. **Delete the generic ML-era opening sentence.** First sentence is
   precious real estate.
4. **Q before A: motivating example → abstract problem → closing
   experiment.**
5. **Describe your method by what it does, not what it doesn't.**
6. **3-sentence minimum paragraphs; balanced sections / subsections.**
7. **Figures alone tell the story AND text alone tells the story.**
   Captions 1-3 lines.
8. **Contribution arrives before page 5/8.**
9. **Anticipate critical questions in the text; if you don't know the
   answer, run the experiment to find it.**
10. **"We" = author-plus-reader.** (Or in the stricter reading: "we the
    authors of this paper.")
11. **No hostage-to-fortune claims** ("most datasets" → "many datasets").
12. **Sin of omission > sin of commission** for uncertain claims.
13. **Opinions labeled as such** ("in our opinion").
14. **Short sentences.** Sophistication in ideas, not syntax.
15. **No intensifiers / vacuous adverbs** (Extremely, Very, Incredibly,
    Completely, Barely, Essentially, Rather, Quite, Definitely, real,
    truly, rich, complex).
16. **Subject-verb agreement, no anthropomorphism** ("algorithms don't
    try, don't think"). Attribute every action.
17. **No passive without subject** ("LSTMs are claimed to X" — claimed
    by whom?).
18. **Cite generously, throughout, and exhaust the references limit.**

## Reviewer-Facing Implications

Lipton is explicit about the reviewer-side payoff for each heuristic:

- Heuristic 7 explicitly notes "reviewers will" skip to figures.
- Heuristic 8: contribution late in paper = no excuse for reviewer
  complaints unaddressed.
- Heuristic 9: anticipate the critical question or get rejected.
- Heuristic 12: a one-line boast can inspire rejection.
- Heuristic 15: intensifiers signal insecurity, not confidence.
- Heuristic 17: cite-likely-reviewers (good karma + read-through).
- Heuristic 19: empty bibliography page = no sympathy for missed citation.

This is reviewer-modeling-as-craft: every heuristic is justified through
the reviewer's reading behavior.

## ML-Paper-Specific Advice (Distinguished from General Writing)

**Heavily ML-specific:**

- Heuristic 7's CV-community-caveat about figure-heavy norms ("the
  computer vision community understandably has a very different
  relationship with figures").
- Heuristic 8's "5-10 papers per conference, 50-100 per year in similar
  areas" — modeled on ICML/NeurIPS reviewer load.
- Heuristic 17's "cite-likely-reviewers" — assumes the small-world ML
  community where reviewers are likely authors of nearby work.
- The comment-section additions ("LSTMs", "GANs", "deep learning has had
  many successes") are ML-vocabulary-specific.

**General-writing crossover:**

- Heuristics 10, 14, 15, 16 overlap with Strunk-White / Pinker's *Sense
  of Style* / Halmos's *How to write mathematics*.
- Heuristic 11 (hostage to fortune) is a general academic-writing
  precept.

## Connection to SiS / CTPC

Direct pre-submission audit map for [[CTPC-KDD-Submission]]:

- **Heuristic 1 (4-bullet abstract):** does the current CTPC abstract
  follow context / what's wrong / contribution / quantitative result?
  The "d̄² ≈ 1 vs d̄² > 20,000" and "64% MSE reduction at 4-day horizon"
  are the bullet-4 quantitative anchors.
- **Heuristic 2 (don't tease):** these numbers should be in the abstract
  AND the introduction, not just in the experimental section.
- **Heuristic 3 (delete generic opening):** SDA's "as the number of
  resident space objects grows..." opening pattern should be audited
  — if it could prepend any SDA paper, replace.
- **Heuristic 8 (contribution before page 5/8):** the CTPC contribution
  (Predictor + Corrector + RTN-decomposition + Student-t calibration)
  needs to arrive early.
- **Heuristic 9 (anticipate critical questions):** the standing
  questions in the agent files (Hamiltonian-NN, ML-Theory, etc.) are
  *exactly* the kind of reviewer-questions Lipton wants pre-empted in the
  paper. The agent council is operationally a critical-question generator.
- **Heuristic 11 (hostage to fortune):** "CTPC outperforms Latent ODE
  baselines" → on what dataset, what horizon, what baseline parameter
  settings? Specifics not just generalities.
- **Heuristic 15 (no intensifiers):** audit the PhyArch / CTPC writeups
  for "extremely", "very", "powerful", etc.
- **Heuristic 17-19 (citations):** the related-work split per
  [[Ramdas-Paper-Checklist]] item 4 + cite-throughout + exhaust-limit
  triple-rule applies.

## Connections

- [[ICML-2022-Review-Form]] — the reviewer Lipton is modeling
- [[ICML-Best-Practices]] — explicitly recommends this post in its
  resource list
- [[Ramdas-Paper-Checklist]] — overlaps on heuristics 14-16 (math
  writing) and 17-19 (bibliography)
- [[Strunk-White-Elements-Style]] — overlaps on heuristics 10, 14-16
- [[CTPC-KDD-Submission]] — direct pre-submission audit target

## Open Questions

1. **Lipton's heuristic-conflict warning** ("Most are just heuristics so
   take each with a grain of salt, especially when they come into
   conflict"): how to resolve when, e.g., heuristic 8 (contribution by
   page 5) conflicts with heuristic 4 (Q before A → may need substantial
   problem setup)? The post doesn't give a meta-rule.
2. **The "agents of interest are papers" stance** (Lipton's later edit)
   — this is a strong claim about authorial voice. The SiS wiki's
   first-person "Bilal does X" frame in some pages doesn't conform; should
   wiki prose be re-aligned, or is the wiki convention different from
   paper convention? (Probably: the wiki keeps its convention; paper
   submissions adopt Lipton's.)

## Sources

- `raw/writing/guides/Heuristics for Scientific Writing (a Machine Learning Perspective) – Approximately Correct.pdf`
  — 17 pages including 21-comment thread. Author: Zachary C. Lipton (CMU),
  posted January 29, 2018.
- Original URL: https://www.approximatelycorrect.com/2018/01/29/heuristics-technical-scientific-writing-machine-learning-perspective/
- Lineage credit: Charles Elkan ("I learned early in my PhD from Charles
  Elkan...").
- Notable comment additions: Tom Dietterich, Daniel Roy, Greg Ver Steeg
  (recommends Pinker's *Sense of Style*).
