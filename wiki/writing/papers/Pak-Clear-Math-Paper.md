---
title: "How to Write a Clear Math Paper: Some 21st Century Tips"
tags: [writing, paper-writing, clarity, structure, references, latex, arXiv, math-writing, micro-tips, macro-tips]
sources:
  - raw/writing/papers/guides/Write_Clear_Math_Paper.pdf
created: 2026-05-21
updated: 2026-05-21
sis_relevance: high
hard_constraint_possible: no
---

# How to Write a Clear Math Paper: Some 21st Century Tips (Pak)

## One-Line Intuition

In the arXiv era, clarity is a competitive advantage — readers skim, cite, and abandon papers in seconds, so your job is not to be perfect but to be unmistakably readable to a diverse, impatient, multilingual audience.

## Why This Matters

Igor Pak (UCLA Mathematics) wrote this guide because existing guides (Halmos, Knuth, Higham, Krantz) were written before arXiv and Google Scholar reshaped how papers are actually read. Modern readers do not read linearly from top to bottom; they skim titles and abstracts on arXiv, scan introductions, jump to main results via PDF links, and decide within seconds whether to invest further. A paper written for the old "devoted reader who starts on page 1" model is being written for an audience that no longer exists. Pak's guide recalibrates for the 21st century reality: short attention spans, non-native English speakers, competitive markets for credit, and the arXiv as the primary publication venue (journals are now largely a post-hoc formality).

For Bilal specifically: SiS papers must compete for attention from reviewers at NeurIPS/ICML/ICLR who are reading dozens of submissions, program officers scanning proposals, and SDA practitioners who have engineering rather than ML backgrounds. All of Pak's advice applies directly.

---

## Core Content

### Section 1: Be Clear! — The Golden Rule

#### 1.1 What Does It Mean to Be Clear?

Pak distinguishes surface clarity (clear phrasing, e.g., "Abelian groups have trivial center" vs. the bloated version) from **deep clarity** — the kind that requires structural, notational, and argumentative choices throughout the entire paper. Most people think clarity is just about phrasing. It is not. It is a whole-paper commitment.

Key example: If you discover on the last page that you used `h` as both a variable and a function throughout the paper, the correct response is NOT to add a disclaimer "In this section, h is a function" at the top. The correct response is to spend 30 minutes going through every instance and renaming consistently. Pak is emphatic: **yes, really, spend the 30 minutes.**

#### 1.2 Being Clear — How Hard Can That Be?

- Being clear takes time and effort. It does NOT come naturally or automatically with experience.
- Noga Alon's answer when asked how he became such a good (and fast) writer: "it gets easier after the first 300 papers."
- The test of commitment to clarity is not when it is easy — it is when fixing it is costly (time, effort, late-stage rewrites).

#### 1.3 Why Be Clear? — The Reader-Centered Argument

Pak reframes clarity as an investment that pays off on the reader's side, not yours:

- **Scenario 1:** Graduate student at a small university, poor English skills, reading your paper. Confused on page 3, he gives up and cites an older, weaker, better-written paper instead. You spent 1 minute not fixing the notation. You lost a significant fraction of your readership.
- **Scenario 2:** Postdoc with 20 papers to check for project relevance. Your notation confusion makes her conclude your paper is irrelevant. She moves on. You lost both the citation and a chance to advance the area.
- **For junior mathematicians:** Clear writing makes it impossible for lazy senior scientists to dismiss your work as ambiguous. "Don't give them one!"
- **For all mathematicians:** Clear writing is a competitive advantage. When two groups independently obtain nearly the same result, the clearer paper gets the credit. "Sometimes it's not about the substance but the presentation."

#### 1.4 Can't Journals Help?

**No.** Copy editors catch linguistics (sentence fragments, missing verbs) but not math clarity. And since most readers find your paper on arXiv anyway — not through journals — you must own the clarity yourself.

#### 1.5 For the Sake of Clarity, Ignore All Rules

This is motivated by Wikipedia's "Ignore All Rules" guideline. When style/grammar rules make the math *less* clear, break them. Example: ending a sentence with a period after a displayed equation can create mathematical confusion (the period looks like part of the formula). In that case, omit the period. Try rewording first; if nothing works, ignore the rule.

---

### Section 2: Where to Start

#### 2.1 Not With This Article — Read Other Literature First

Pak recommends starting with:
- **Halmos et al.** — classic essays on mathematical writing
- **Higham** — *Handbook of Writing for the Mathematical Sciences* (SIAM)
- **Knuth** — *Mathematical Writing* (MAA)
- **Berndt, Goldreich, S. P. Jones** — more recent guides
- **Terry Tao's blog** — further essays and resources

#### 2.2 Read a Good Guide on Writing Nonfiction

Pak strongly recommends **Zinsser's** *On Writing Well* — calls it foundational, compares it to good makeup: it is the foundation, not the decoration.

Key Zinsser quotes Pak excerpts (p. 80):
- "Keep your paragraphs short. Writing is visual — it catches the eye before it has a chance to catch the brain."
- "Newspaper paragraphs should be only two or three sentences long."
- "But don't go berserk. A succession of tiny paragraphs is as annoying as a paragraph that's too long."

**Pak's calibration:** Native English speakers should read Zinsser first, then mathematical guides. Non-native speakers: read Halmos and short pieces first, come back to Zinsser with more experience. Read Zinsser again a few years later — you will find new things.

#### 2.3 Why Do We Need a New Guide?

Old guides assume readers read linearly and papers are expected to last decades. Modern reality:
- arXiv has created a massive proliferation of papers; nobody reads all relevant work
- Different readers engage at different depths: title-only, abstract-only, introduction-only, main-results-only, full read
- LaTeX/arXiv is now universal; technical TeX advice in old guides (like Knuth's) has become both more important and more stale
- The emphasis shifts from *perfect* to **clear** — because papers are no longer expected to be interesting for decades; short-term goals dominate

---

### Section 3: Macro Tips — Paper Structure

Pak uses the "Matryoshka doll" metaphor: a good paper is layered — super-brief summary → brief summary → full facts. The layered structure is: **Title → Abstract → Introduction → Main Part → Final Remarks**.

#### 3.1 Structure of the Paper

The standard progression exists for a reason: each layer hooks the next level of reader investment. Modern practices require updating old guidelines for each part.

#### 3.2 Title

- **Super important.** Think about it for a long time. Try different versions on colleagues. Think again.
- Should NOT be too long, too short, too vague, or too generic (e.g., "On some problems in group theory").
- The title is "the first approximation to contents of your paper."
- **Naming trick:** introduce a cumbersome new object class and give it a proper name (e.g., "Munro permutations" inspired by Alice Munro's book). Then the title becomes "Asymptotic analysis of Munro's permutations." Readable, memorable. Trade-off: others may use the name and never mention you.
- Pak's MathOverflow advice on titles (verbatim excerpt): "You should emphasize not the length but the content. If you prove that all tennis balls are white make the title 'All tennis balls are white'. If you prove that some tennis balls are white, title your note 'On white tennis balls', or 'New examples of white tennis balls', or whatever. If your note is a new simple proof, make the title 'Short proof that all tennis balls are white'. If there was a conjecture that all tennis balls were white and you found a counterexample, use 'Not all tennis balls are white'. If you study further properties of white tennis balls, use 'A remark on white tennis balls'."
- For surveys: emphasize the survey nature regardless of length ("A survey on white tennis balls"). A title like "A short survey on tennis ball colors" signals *brief and incomplete*, not short in length.

#### 3.3 Abstract

- Think of it as a short MathSciNet summary (facts-only, no connections to other work unless absolutely necessary).
- State key results first, briefly mention existence of others (generalizations, etc.), no precise statements needed.
- No details, no citations (unless "We disprove a conjecture stated by the author in [Pak12]" where precision matters).
- **Rule of thumb:** lines in abstract = 0.3–0.5 × number of pages. A 10-line abstract for a 10-page paper is too much.
- Large fraction of MathSciNet reviews are just the abstract, so make it "clear, precise, plain and uninventive."

#### 3.4 Table of Contents

Don't include it unless paper is over 60 pages. Adobe Reader has this feature built in; most people read PDFs anyway.

#### 3.5 Introduction — The Hardest Section

- The only part most readers will actually read completely.
- If you have a senior coauthor, ask them to write it. If not, ask a senior colleague to read and comment on a draft.
- **Write a first draft of the Introduction early** (to know what is in the paper), then **completely rewrite it** after the rest is written.
- Let the paper "stew for a week" before showing it to trusted colleagues. After their comments, rewrite the Introduction again, perhaps with a new emphasis.
- Rota's quote (Pak calls it "half-right"): "Nowadays reading a mathematics paper from top to bottom is a rare event. If we wish our paper to be read, we had better provide our prospective readers with strong motivation to do so. A lengthy introduction, summarizing the history of the subject, giving everybody his due, and perhaps enticingly outlining the content of the paper in a discursive manner, will go some of the way towards getting us a couple of readers."
- Pak's addendum: "nowadays some people don't even have patience to read a long introduction." Solution: write a Rota-style lengthy introduction, but then extract a few pages and move them to **Final Remarks**. Or use a **Foreword** (see §3.6).

**What to include in the Introduction:**
1. Set up the problem and state the main results.
2. Include only the few technical definitions strictly needed for the result statements.
3. If resolving a conjecture or question, state it with attribution.
4. **Aim to have your first theorem on page 1, or at worst page 2.**
5. To get around cumbersome theorem statements: skip the main theorem and instead present "interesting, nontrivial, but easy-to-state corollaries" — the paper-passers-by will remember these and repeat them to others.
6. In the final paragraph or subsection of the Introduction, outline the structure of the paper in your own words (no table of contents needed — section links in Adobe Reader serve as navigation).

**What NOT to include in the Introduction:**
- Technical definitions, examples, big figures for special cases.
- Use forward references instead: "(see Figure 5.1)" or "(see the exact definition in §3.4)".
- Rota's "giving everybody his due" in the Introduction — there are too many relevant papers now. Instead, give only directly relevant history. The detailed credit goes in Final Remarks.

#### 3.6 Foreword

Use a Foreword if:
- Introduction is over 4 pages (likely means many results or a broad audience area), OR
- You just want to write a great first sentence (Pak encourages this for ALL papers).

The Foreword functions as a **nontechnical introduction to the Introduction**. Use it for:
- Highly literary description of what you are doing (if memorable enough, gets quoted in unrelated papers).
- Big picture motivation, NSF project outline, motivating examples from other fields, philosophical principles.
- This is "your only place to shine" as a writer in an otherwise rigid format.

Pak's contrast with Zinsser: Zinsser says editors often throw away the first 3–4 paragraphs of an article and start with where the writer "begins to sound like himself or herself." Math writers have the opposite opportunity — they need only be good writers in the first few lines. "Skipping out on this is a missed opportunity."

Nothing is less inspiring than a paper that starts "Let G = (V, E) be a loopless graph on n vertices."

#### 3.7 Final Remarks — The Endnotes Section

- This section is the **least understood** but actually functions as an expanded footnote / endnote section (like endnotes in humanities monographs or literary novels).
- The Final Remarks section must be **neatly divided into untitled subsections**, each 1–2 paragraphs to a page at most (use `\subsection{}` to generate a number without a title).
- Subsections should be **ordered by decreasing importance** (not by logical order — this is not meant to be read in order).

**Typical subsection ordering:**
1. Expanded history of the subject; giving everybody their due.
2. Where do you go from here? Future directions, potential applications.
3. Your more speculative conjectures.
4. Other people's speculative conjectures.
5. Anything else not included in the main text.

**The placeholder technique:** While writing the Introduction or main body, whenever you feel the need for more context, explanation, or related references — create a placeholder subsection in Final Remarks. When done with the paper, go over everything and insert forward references: "(for more on this, see §6.1)", "(cf. §6.4)", "We postpone the discussion until §6.8". The interested reader clicks the link; the casual reader skips.

**Managing Final Remarks size objections from referees/editors:**
- Extract lengthy subsections into separate "Open Problems" or "Historical Overview" sections.
- Remove some from the journal version but keep the full version on arXiv.
- Set the font of Final Remarks to `\small` or even `\footnotesize`. "Human eye is a fantastic instrument."
- Since arXiv is easily updateable, you can continue adding subsections to Final Remarks post-publication. This gives the paper a "dynamic semi-survey" quality.

#### 3.8 Acknowledgements

- Rota advises "lavish acknowledgements" — Pak agrees with modifications.
- Structure:
  1. **Thank everyone** who discussed the results, in alphabetical order, by name (use the name they commonly go by, not necessarily their full legal name; check emails for sign-off names).
  2. **Single out** people who gave especially helpful suggestions — explain what specifically they contributed (e.g., "We are especially thankful to Adam Smith for informing us about his invisible hand Lemma; this result played a crucial role in our laissez-faire equilibrium analysis in Section 3, leading to a much shorter proof of Theorem 3.4.").
  3. **Thank institutions** that hosted you and facilitated collaboration.
  4. **Thank granting agencies** last.
- Do NOT email for permission before thanking, unless using private information (e.g., a private conjecture). If someone asks to be removed, do so.

---

### Section 4: References — "The Primary Vehicle for Our Goal"

#### 4.1 Why So Important?

Pak self-quotes from his blog: Citations are "the tiny little thing we call" the mechanism by which the entire system of mathematical credit works. "If we are uncited, ignored, all hope is lost." Everyone pays attention to how and where they are cited. Treat this carefully: low cost to the author, potentially large positive or negative effect on others.

#### 4.2 How to Cite a Single Paper — The 12-Level Taxonomy

Pak provides a detailed, ranked taxonomy of citation forms from strongest to weakest attribution. This is one of the most practically valuable sections in the entire document.

1. **"Roth proved Murakami's conjecture in [Roth]."** — Clear. Roth's proof is in [Roth].
2. **"Roth proved Murakami's conjecture [Roth]."** — Roth proved it, possibly in a different paper than [Roth], but [Roth] is likely a definitive version.
3. **"Roth proved Murakami's conjecture, see [Roth]."** — [Roth] can be anything from the original paper to a survey Roth wrote. "see" without "also" signals Roth's own writing.
4. **"Roth proved Murakami's conjecture [Roth], see also [Woolf]."** — Woolf made an important contribution, perhaps extending or fixing gaps.
5. **"Roth proved Murakami's conjecture in [Roth] (see also [Woolf])."** — Woolf likely has a complete proof, possibly fixing minor errors in [Roth].
6. **"Roth proved Murakami's conjecture (see [Woolf])."** — [Woolf] IS the definitive version, e.g., the standard monograph.
7. **"Roth proved Murakami's conjecture, see e.g. [Faulkner, Fitzgerald, Frost]."** — Result is important and confirmed in several books/surveys. Original proof may have been long, incomplete, or inaccessible.
8. **"Roth proved Murakami's conjecture (see e.g. [Faulkner, Fitzgerald, Frost])."** — Result is probably classical; these books all state/prove it. "Neither the author nor the reader will ever bother to check."
9. **"Roth proved Murakami's conjecture.^7 Footnote 7: See [Mailer]."** — Author probably never read [Mailer]. Or [Mailer] states it without proof or reference. Author visibly annoyed by the ambiguity.
10. **"Roth proved Murakami's conjecture.^7 Footnote 7: Love letter from H. Fielding to J. Austen, dated December 16, 1975."** — The letter likely exists with an outline of proof. Author may or may not have seen it.
11. **"Roth proved Murakami's conjecture.^7 Footnote 7: Personal communication."** — Roth emailed the author or mentioned it after a talk. Proof may or may not be correct; paper may or may not be forthcoming.
12. **"Roth claims to have proved Murakami's conjecture in [Roth]."** — [Roth] has a well-known gap that was never fixed; author would rather not go on record about this.

#### 4.3 How to Cite a List of Papers

**Never** write "See [2–19] for some relevant work." This helps neither readers nor authors. Instead, describe each paper's contribution individually:
- Start with the most important reference.
- Describe what each paper does, in relation to your work.
- Stop when you are tired.
- This type of survey-style citation prose goes into **Final Remarks**, not the Introduction (Introduction should cite only 2–3 most directly relevant papers at most).

#### 4.4 Where to Cite a Paper?

- **Introduction:** only the most relevant papers.
- **Final Remarks:** everything else.
- **Main body of paper:** ONLY when using a result as a lemma (where a precise reference is required). Do NOT elaborate on it there; story goes in a Remark after the proof or in Final Remarks.
- **Never** cite in the middle of a proof flow — breaks the argument.

#### 4.5 Forming Your Reference

When quoting a specific result from a monograph or paper >5 pages: include a **specific location** — theorem number or section number, not just page number (arXiv pagination changes; sections are stable). Use `[A, §3.1]` format.

#### 4.6 Style of References

- **Do NOT use BibTeX** unless you are an advanced user who knows exactly what you are doing. MathSciNet citations are bloated, inconsistent in style, and wrong on author names (Paul Erdős vs. Erdős, P.).
- Pick a style and stick to it. Make each reference as concise as possible.
- Abbreviate first names, omit book series and issue numbers.
- For longer papers: use alphanumeric style `[SY09, Woo96]` so readers know name and date.
- For unpublished: use `[Con17+]`.
- For 5+ authors: use `[A+13]` not `[ABCDEF13]`.
- Same name/date: `[Tra15a]`, `[Tra15b]`.
- **Do NOT emulate Knuth.** Knuth aims for perfection; you need clarity. Avoid fully expanded second names (confusing), Chinese names written in characters, Russian titles in Cyrillic.
- For unpublished papers: include the arXiv number or a free-version link. Use `hyperref` and `url` packages; use https://tinyurl.com or https://bitly.com to shorten long URLs.

---

### Section 5: Micro Tips

#### 5.1 Basic Grammar, Syntax, Punctuation

Pak defers to existing guides: Goldreich, Halmos, Poonen, West for short treatments; Higham and Krantz for thorough ones. He does not repeat rules here.

#### 5.2 Don't Be Pedantic

Serre famously advises against writing Q ⊂ R because there are multiple embeddings. Pak disagrees: "when it comes to standard or easy notions, the extra explanations are distracting and make the paper less clear." Unless you are a member of Bourbaki, ignore this advice from Serre.

Similarly: if you are discussing domino tilings, do not define what "simply connected" means. Draw a picture and get on with your math.

#### 5.3 Downshift Your Style

Berndt advises writing at the literary level of Shakespeare, Rowling, and Tolstoy. Pak strongly disagrees: your audience includes readers with limited English, short attention spans, and no patience. It is perfectly OK to be repetitive and have 10 "therefore's." It is better to be clear than to vary style and use difficult words like "henceforth."

**Pak's style rules (all of §5.3):**

1. **Use present indefinite and occasionally past indefinite tenses** in the main body. Outside Introduction and Final Remarks: only these tenses.
2. **Write short sentences.** If you must use longer sentences, separate each clause with a comma.
3. **Comma rule (Pak's own):** "Commas exist to make the sentence structure clearer, rather than to indicate where the English speakers make a pause when reading." Place commas wherever they clarify structure. Ignore Oxford comma debate. Ignore Strunk & White's comma rules.
4. **Do NOT use a semicolon** — it implies a logical connection between clauses best made explicit.
5. **Long dashes:** leave spaces at both ends ("obsessed by this fear" — Zinsser's quote style), since they are distracting otherwise.
6. **Always use the most common spelling** in Standard American English. Do not use British spellings (coöperation, colouring) even if you prefer *The New Yorker* or the *BBC*. Use Google Search to break ties (dominoes vs. dominos).
7. **Mathematical terms:** once you introduce a term ("nice graph"), do not use variations ("niceness of graphs imply..."). Stick to the basic form always.

#### 5.4 LaTeX Tips

**Create a ton of macros.** For everything. Examples: `\al` for `\alpha`, `\rT` for `\textrm{T}`, `\lra` for `\leftrightarrow`. Use the same macros in ALL your papers. Benefits:
- Faster to type.
- If you want to change notation, you change one macro definition and the whole paper updates instantly.
- Check if a letter is used with another meaning: just comment out the macro definition and see if the file compiles.
- arXiv keeps LaTeX files — you can copy macros from other people's papers.

**Letter selection advice:**
- Letters with "tails" tend to be functions: f, g, φ, φ, ζ, ξ, η, ρ.
- Early alphabet: a, b, c, d, e — constants.
- Late alphabet: x, y, z — variables.
- Middle: i, j, k, ℓ, m, n — always integers.
- Avoid: Ξ, ι, ϖ, ι, ȷ, ϰ (look weird); κ and ν (look too much like k and v); ∅ (looks like computer zero — use ∅ instead); always use ℓ not l, and ε not ε.
- Create mnemonic systems for what letters mean and stick to them across all your papers.
- For notation unsettled while writing: use a macro placeholder. Play with different fonts once the paper is finished to ensure similar-looking letters mean clearly different things.
- **Avoid Gothic fonts** (𝔊, 𝔖, 𝔍, 𝔗): impossible to copy on a board.
- Feel free to use unusual LaTeX symbols (∗, ∗∗, ◇, ⊗, ⊛, ♡) to tag key equations. "Why be constrained by old conventions?"

#### 5.5 Do NOT Trust LaTeX

LaTeX layouts formulas automatically — there is a tendency not to second-guess it. This is wrong. LaTeX imperfections can create ambiguity. Example: Φ(2a+c)Φ(c−2a)Φ(a)Φ(3a) / [Φ(a+c)Φ(c−a)Φ(2a)Φ(4a)] looks like Φ is a strange ⊕-like symbol separating linear forms. Inserting `\,` between terms makes clear you are taking a product of multiple Φ(·) values.

Similarly: the partition 12+10+10+2+1 written in short form becomes 1210²21, which looks like a different partition 12·10²·1. Inserting spaces solves it: 12 10² 2 1.

**Always check the PDF output with fresh eyes, treating yourself as a reader who does not know what the formula means.**

#### 5.6 Figures

- Make lots of them, but **make them small**. 3–5 cm is enough for most figures.
- "Human eye is a very good instrument."
- Do NOT make BIG figures — they break the natural flow and distract the reader.
- If you need many details: create several copies of the same picture aligned next to each other, each with different levels of detail.
- If you MUST have a huge figure (over half a page): move it to an appendix or your website, and add a link directing the reader there.

---

### Section 6: What to Do When You Are Done

#### 6.1 You Are Never Really Done

- When you finish writing: **let it stew for a week or so.** Come back, reread everything with fresh eyes for typos, bad wording, imprecise statements.
- Do NOT post to arXiv right away. Put it on your personal website first. Email a few colleagues. They will give you mathematical advice AND catch typos.
- A week later, THEN post to arXiv. Keep updating arXiv whenever you find/fix errors.
- **Exception:** If the published journal version has a wrong proof or a missing key reference (rediscovering somebody's result): that warrants a separate file titled "Erratum" or "Acknowledgement of attribution." Do not silently whitewash.

#### 6.2 Advertise and Popularize

- If you give a recorded talk: link from your website to the talk alongside the PDF and slides.
- **Blog about it** — explain the gist in accessible terms, link back to the paper.
- **Write a survey** on the subject. More work, but you learn a great deal and place your results in context. Per Rota: "You are more likely to be remembered by your expository work."
- **Extract a simple special case:** find the simplest nontrivial version of your result that is stateable for college students, make it into an olympiad problem. Submit to IMO committee, Putnam committee, USAMO. "They are always looking for good and unusual problems."
- If the special case is too hard for Putnam but substantially simplifies the argument: write it up as a separate note, submit to *Amer. Math. Monthly*, *Math. Intelligencer*, or *Math. Magazine*.

#### 6.3 Rewrite and Republish

- If your published paper is being ignored because it is unclear: **rewrite it.** Simplify arguments, generalize slightly, find new examples or applications. Publish this new version, even just on arXiv.
- When a later paper overgeneralizes and the arguments become cumbersome: it is OK to start a new paper with a proof of the known theorem "for completeness" and then explain how the proof needs to be modified for the generalization. "You get the best of both worlds: a clear presentation of your own old result and a clear pathway to your new generalization."

---

## Key Verbatim Anchors

All quotes below are from the PDF; page numbers reference the PDF's internal pagination.

1. **The golden rule** (p. 1): "This is the *golden rule*, really. It's absolutely paramount."

2. **Clarity is reader-centered** (p. 2): "Being clear is not about you! You must think of the reader and how they will read your paper."

3. **Competitive advantage** (p. 2): "*Clear writing will give you a competitive advantage.* It is often the case that the same or nearly the same result is obtained in several papers. If your paper is clear and your competitors' are not, you will get the credit."

4. **Ignore all rules** (p. 3): "For the sake of clarity, ignore all rules! [...] When the rules of style and grammar make math unclear, you should simply ignore these rules."

5. **Abstract size rule** (p. 5): "As a rule of thumb, the number of lines in the abstract should be at 0.3–0.5 times the number of pages. An abstract with 10 lines for a paper of 10 pages looks way too excessive."

6. **Introduction is hardest** (p. 6): "This is the hardest section to write. It's probably the only part of your paper that will be read by all but a few most devoted readers."

7. **First theorem placement** (p. 6): "Do aim to have your first theorem on the first page, or at worst on the second page."

8. **Final Remarks as endnotes** (p. 8): "*My point is simple*: Final Remarks section must play exactly the same role [as endnotes in serious monographs]."

9. **Citations are the mechanism** (p. 10): "But all this hinges on a tiny little thing we call citations. They tend to come at the end, sometimes footnote size and is the primary vehicle for our goal. If we are uncited, ignored, all hope is lost."

10. **BibTeX warning** (p. 13): "Do NOT use BibTeX, unless you are an advanced user and know exactly what you are doing."

11. **Comma rule** (p. 14): "Commas exist to make the sentence structure clearer, rather than to indicate where the English speakers make a pause when reading."

12. **Downshift your style** (p. 14): "Your audience will likely include readers with limited English, short attention span and no patience. Thus it is perfectly ok to be repetitive and have ten therefore's and be clear, rather than vary the style and have difficult words like 'henceforth'."

---

## How a Writing Agent Should Use This

This document is a **primary action guide** for the paper-writing agent, relevant in the following modes:

### Mode 1: Pre-draft planning
- Use §3.2 (Title) to generate and evaluate candidate titles. Apply the "tennis ball test": what exactly did you prove, and what is the most content-dense short phrase that describes it?
- Use §3.3 (Abstract) to size the abstract: count current pages, compute 0.3–0.5× to get target line count.
- Decide whether paper needs a Foreword (§3.6): is Introduction >4 pages? Does the area span multiple sub-fields?

### Mode 2: Introduction drafting
- Apply §3.5 checklist: (a) first theorem on page 1 or 2, (b) only a few technical definitions needed for the theorem statement, (c) conjecture attribution if applicable, (d) structural outline in last paragraph.
- Draft easy-to-state corollaries as "advertisements" for results that are technically complex to state.
- Move all "giving everybody his due" to Final Remarks placeholder.

### Mode 3: Final Remarks structuring
- Use §3.7 protocol: untitled `\subsection{}` blocks, decreasing importance order, placeholder technique.
- Check all points in the Introduction where Bilal wanted to say more — convert to Final Remarks forward references.

### Mode 4: Citation audit
- Walk through every citation in the paper and classify it using Pak's 12-level taxonomy (§4.2). Any level 9–12 citation is a red flag — either elevate the reference quality or acknowledge the uncertainty explicitly.
- Check that the Introduction cites at most the 2–3 most directly relevant papers (§4.4).
- Verify reference format: alphanumeric for papers >30 references, section-based locators not page numbers (§4.5).

### Mode 5: Style pass (micro)
- Flag all semicolons for removal or replacement (§5.3).
- Check all tenses in main body — should be present indefinite (§5.3).
- Check comma usage: are commas clarifying structure or just indicating breath pauses? (§5.3)
- Verify all mathematical term variations: if "port-Hamiltonian" is introduced, does the text ever say "port-Hamiltonianity" or "PHN-ness"? Find and fix (§5.3).
- Check all figures: are any >half a page? (§5.6)

### Mode 6: LaTeX/notation check
- Verify macro consistency across sections (§5.4).
- Spot-check every multi-factor formula in the PDF output for spacing ambiguity (§5.5).
- Check for Gothic fonts in notation (§5.4).

---

## Connection to SiS / CTPC

Pak's advice maps directly to the challenges Bilal faces writing SiS and CTPC papers:

**Title and audience:** CTPC papers will be submitted to venues with reviewers from ML (NeurIPS/ICML), control theory (CDC/ACC), and SDA (AMOS/AAS). The title must be readable by all three communities. Apply Pak's "first approximation to contents" rule: not "On some learning methods for orbital prediction" but something like "Physics-Constrained Residual Learning for SGP4 State Correction with Certified Dissipation" — dense but content-bearing.

**First theorem on page 1:** The CTPC architecture paper's main result (e.g., a formal guarantee on Ḣ ≤ 0 being exactly enforced, or a bound on propagation error) should appear in the Introduction with a simplified, easy-to-state form. Even if the full theorem requires 5 definitions, a corollary version can be stated in 2 sentences.

**The notation consistency problem:** Physics-ML papers are especially prone to the variable/function confusion Pak discusses (§1.2). In CTPC papers: H is used as both Hamiltonian scalar and covariance matrix in different subsections. This MUST be caught and resolved before submission — not papered over with a disclaimer.

**The 12-level citation taxonomy:** SiS papers will cite SGP4 (old NASA documentation), HNN (Greydanus et al.), Port-Hamiltonian NN (van der Schaft, Desai), and arXiv preprints. Pak's taxonomy helps Bilal choose the right citation strength for each: "SGP4 is the baseline predictor [Vallado, 2006]" (level 1) vs. "dissipative structure has been studied, see e.g. [van der Schaft, Desai]" (level 7).

**Final Remarks as SDA context dump:** The SDA context (TLE data, CDM, Space-Track pipeline, CARA) is important for practitioners but breaks the flow of a math-ML paper. Final Remarks is exactly the right place for a subsection titled "Practical SDA Deployment Notes" — written using Pak's placeholder technique during drafting.

**Non-native English reader base:** SiS papers may be read by practitioners at Space Force, ESA, or JAXA whose English fluency varies. Pak's "downshift your style" rule (§5.3) is not stylistic conservatism — it is engineering for actual readership. Use short sentences. Repeat notation reminders. Tolerate ten "therefore's."

**arXiv-first strategy:** Pak's entire framing assumes arXiv is the primary distribution channel. SiS should post to arXiv before (or simultaneous with) conference submission to establish timestamp priority, and keep arXiv versions updated with Final Remarks additions as the project evolves.

---

## Connections

- [[Krantz-Primer-Mathematical-Writing]] — Pak cites Krantz as a thorough reference for grammar and punctuation; contrasts with his own style philosophy
- [[Goldreich-How-To-Write-Paper]] — Pak cites as a more recent guide; Goldreich's advice on CS theory papers uses "Conclusions" differently from math's "Final Remarks"
- [[Bertsekas-Ten-Rules-Mathematical-Writing]] — closely related rules-based guide for mathematical writing in an ML/optimization context
- [[HKU-Guide-Writing-Mathematics]] — complementary guide on mathematical exposition
- [[Milne-Tips-For-Authors]] — another math-specific writing guide in the same tradition
- [[Peyton-Jones-Research-Paper]] — CS/ML analogue; Peyton Jones's "how to write a great research paper" talk covers similar ground for conference venues
- [[Ramdas-Paper-Checklist]] — practical checklist form of many rules Pak discusses; use as a companion audit tool
- [[Lipton-Writing-Heuristics]] — ML-specific heuristics; overlaps with Pak's micro-tips but geared toward deep learning papers
- [[Freeman-Writing-Papers]] — covers similar Introduction/abstract structure from a different angle
- [[Whitesides-Writing-A-Paper]] — Whitesides's reverse-outline method is a concrete technique for achieving Pak's "first theorem on page 1" goal
- [[Strunk-White-Elements-Style]] — Pak explicitly contradicts Strunk & White on comma usage (§5.3); reading both in parallel is instructive

---

## Open Questions

1. **Foreword vs. broader introduction:** Pak recommends a Foreword for multi-sub-area papers. CTPC spans orbital mechanics, ML, and control theory. What is the right threshold for using a Foreword vs. a well-structured Introduction with a short motivational first paragraph?

2. **Citation taxonomy for preprints:** Pak's taxonomy was written for published papers. How should level 1–3 citations be handled when the authoritative source is an arXiv preprint that may change? What citation form signals "this is the definitive preprint version we rely on"?

3. **Macro systems across multi-author papers:** Pak recommends using the same macros in all your papers. In collaborative papers (Bilal + Iowa State advisors + SiS co-founders), how do macro systems get harmonized? Does one author's macro file become the canonical one?

4. **Final Remarks size in ML venue papers:** ML venues (NeurIPS, ICML) have strict page limits (8–9 pages + references), and reviewers rarely read appendices deeply. How does Pak's "Final Remarks as endnotes" structure translate when you have at most 1–2 pages of Final Remarks before hitting the limit? Should the SDA deployment context go in a supplementary appendix instead?

5. **Advertising and popularization in SDA context:** Pak recommends blog posts, surveys, and accessible special cases. The SDA community reads AMOS proceedings and AAS/AIAA conference papers, not arXiv. What is the right popularization channel for SiS results that are primarily presented at ML venues?

---

## Sources

- `raw/writing/papers/guides/Write_Clear_Math_Paper.pdf` — full document, 18 pages, Igor Pak (UCLA Mathematics). All sections read in full. Key sections: §1 (clarity philosophy, pp. 1–3), §3 (macro structure tips, pp. 4–9), §4 (references, pp. 9–13), §5 (micro tips, pp. 13–16), §6 (post-completion, pp. 16–17).
