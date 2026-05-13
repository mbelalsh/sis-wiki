# Writing Corpus Batch Ingest — Completion Report

**Date:** 2026-05-13
**Batch:** 14 sequential ingests of `raw/writing/*.pdf` → `wiki/writing/*.md`
**Tagged uniformly:** `sis_relevance: high | hard_constraint_possible: no`
**Per-page structure:** verbatim quotes / numbered actionable rules / reviewer
implications / ML-specific advice / SiS-CTPC connection / wikilinks
**Triggering instruction:** Bilal's batch request, executed sequentially with
no inter-step confirmation.

---

## 1. Ingest Outcomes (All 14 Succeeded)

| # | Source PDF | Wiki Page | Status |
|---|---|---|---|
| 1 | `ICML 2022 Review Form.pdf` | `ICML-2022-Review-Form.md` | ✅ |
| 2 | `paper_writing_aaditya_ramdas.pdf` | `Ramdas-Paper-Checklist.md` | ✅ |
| 3 | `best_practices_icml.pdf` | `ICML-Best-Practices.md` | ✅ |
| 4 | `1908.03648v1.pdf` (Lipton) | `Lipton-Writing-Heuristics.md` | ✅ |
| 5 | `crafting-ml-papers.pdf` (Langley) | `Crafting-ML-Papers.md` | ✅ |
| 6 | `Simon-Peyton-Jones-research-paper.pdf` | `Peyton-Jones-Research-Paper.md` | ✅ |
| 7 | `1612.04888v1.pdf` (Krantz) | `ArXiv-1612-Writing-Guide.md` | ✅ |
| 8 | `Ten Rules for Mathematical Writing — Bertsekas.pdf` | `Ten-Rules-Mathematical-Writing.md` | ✅ |
| 9 | `guide_to_writing_mathematics.pdf` (HKU) | `Guide-Writing-Mathematics.md` | ✅ |
| 10 | `writing-cvpr2020.pdf` (Freeman) | `Freeman-Writing-Papers.md` | ✅ |
| 11 | `how-to-write1.pdf` (Pak) | `How-To-Write-1.md` | ✅ |
| 12 | `re-writing.pdf` (Goldreich) | `Rewriting-Guide.md` | ✅ |
| 13 | `Tips for Authors -- J.S. Milne.pdf` | `Milne-Tips-Authors.md` | ✅ |
| 14 | `StrunkWhite.pdf` | `Strunk-White-Elements-Style.md` | ✅ |

No `ingest-failed` log entries — all 14 ingests completed cleanly.

---

## 2. Identified Unknown Documents

Three corpus PDFs were filed under generic names and required identification via
title-page direct read + cross-confirmation against [[ICML-Best-Practices]]
resource list:

1. **`1612.04888v1.pdf`** → **Steven G. Krantz**, *A Primer of Mathematical
   Writing* 2nd edition (arXiv:1612.04888v1, December 2016). 6-chapter book.
   Cross-confirmed by ICML-Best-Practices entry "Steven G. Krantz: A Primer of
   Mathematical Writing."

2. **`how-to-write1.pdf`** → **Igor Pak** (UCLA Mathematics), *How to Write a
   Clear Math Paper: Some 21st Century Tips*. Identified via title page
   line 1. Cross-confirmed by ICML-Best-Practices entry "How to write a clear
   math paper by Igor Pak."

3. **`re-writing.pdf`** → **Oded Goldreich** (Weizmann Institute of Science),
   *How to Write a Paper* (March 2004, last revised November 8, 2015). File is
   misnamed — the actual title is *How to write a paper*, not "re-writing."
   This is the revised version of Goldreich's earlier essay "How NOT to write
   a paper" (Spring 1991, revised Winter 1996). Cross-confirmed by
   ICML-Best-Practices entry "Oded Goldreich: How to write a paper?"

Side identification: `paper_writing_aaditya_ramdas.pdf` confirmed as
**Aaditya Ramdas** (CMU Statistics + ML); `1908.03648v1.pdf` confirmed as
**Zachary Lipton** (CMU); `Tips for Authors -- J.S. Milne.pdf` confirmed as
**J. S. Milne** (number-theorist, jmilne.org/math/tips.html);
`crafting-ml-papers.pdf` confirmed as **Pat Langley** (DaimlerChrysler,
ICML 2002 invited talk).

---

## 3. Recurring Themes Across the Corpus

### 3.1 Reader-centricity is the universal principle

Every guide — from Strunk-White's 1918 prose-style rules to Goldreich's
theoretical-CS reader model to Pak's diverse-readership arXiv-era stance to
Freeman's CVPR-tutorial reviewer POV — converges on **the reader's needs, not
the writer's preferences, drive every decision.** The five "writer-centric"
symptoms in Goldreich §3.1 (checklist phenomenon, obscure generality,
idiosyncrasies, lack of hierarchy, Talmud-ism) appear under different names in
nearly every guide.

### 3.2 The Matryoshka / inverted-pyramid structural prescription

Pak §3.1, Peyton-Jones (Adelson 4-point structure), Freeman's slide-arc, ICML
Best-Practices §3-4, and Strunk Rule 10 all prescribe **brief summary → longer
summary → complete facts**. The reader funnel (Peyton-Jones: 1000 → 100 → 10
→ 3 readers; Pak: title-only → abstract-only → skim → intro-only → results) is
the same principle stated from the readership side.

### 3.3 "Omit needless words" is the universal compression rule

Strunk Rule 13 explicitly. Freeman slide 17 quotes it directly. Lipton on
filler phrases. Bertsekas's "small rules". Pak on "clear not perfect" (cut to
clarity, not to perfection). Goldreich on "minimize new concepts."
Milne (inverted): Tip 1 ("never explain") = anti-pattern; real advice is the
ruthless cut.

### 3.4 Active voice + positive form

Strunk Rules 11, 12. Lipton heuristics on voice. Freeman slide guidance.
HKU §3 on verb forms. The convention: lead with the agent (we, the model,
the network), state predicates affirmatively. The exception: passive is
acceptable in procedural method descriptions — but never for the contribution
claim itself.

### 3.5 Numbered evaluation rubrics are operational

[[ICML-2022-Review-Form]] gives the formal rubric. [[Ramdas-Paper-Checklist]]
gives the pre-submission yes/no audit (21 questions). [[ICML-Best-Practices]]
gives the prescriptive ~40-item checklist. The implication: a paper passes if
and only if it satisfies the union of these checklists. Bilal's
[[CTPC-KDD-Submission]] audit should consume these three pages as the
operational triage.

### 3.6 Parallelism and notation consistency

Strunk Rule 15 (co-ordinate ideas in similar form). Bertsekas Rule 4 (consistent
notation). HKU §3 (singular/plural agreement, verb agreement). Milne Tip 3
(variation of definition = anti-pattern). All converge on **one form per
meaning, applied uniformly.**

### 3.7 Internal cross-reference rigor

Ramdas item 7 (refer to assumptions by number). ICML Best-Practices 9.9 (cite
by specific theorem number, not whole book). Milne Tip 6 (vague references =
anti-pattern). Pak §3.2 (memorable naming aids citation). The implication: every
internal reference is by specific number or named object, never gestural.

### 3.8 Conflicts to resolve case-by-case

- **Theorem-numbering convention:** [[Ramdas-Paper-Checklist]] item 7 says
  "Number each type separately"; [[Milne-Tips-Authors]] Tip 7 inverts (says
  separate-numbering = anti-pattern). Resolution for SiS KDD '26 (~9 pages,
  stat-ML community): follow Ramdas (separate numbering).
- **Sentence-end emphasis vs paragraph-start emphasis:** [[Strunk-White]] Rule
  18 (emphatic words at end of sentence) vs newspaper-style /
  [[How-To-Write-1]] Matryoshka (start with the punch). Resolution: paragraph
  leads with contribution (Matryoshka); sentence ends with emphatic words
  (Strunk). Different scales of structure.
- **Rule-following stance:** [[Strunk-White]] is prescriptive-rule-based.
  [[How-To-Write-1]] §1.5 "Ignore all rules for clarity" is the Wikipedia-IAR
  counterweight. Resolution: rules are the default; ignore them only when
  clarity demonstrably requires it.

### 3.9 Field-specific calibrations

- **TCS alphabetical authorship** (Goldreich §4.1) does NOT apply in ML —
  ML follows contribution-ordered authorship.
- **Survive-decades framing** (Strunk + Bertsekas + Krantz era) does NOT apply
  in ML (Pak §2.3) — ML papers' half-life is 2-3 years.
- **Math-density vs prose-density:** HKU and Bertsekas treat math-writing as
  primarily prose-with-embedded-math. ML papers naturally permit higher
  symbol-density in methods sections. The advice scales but doesn't transfer
  one-for-one.

### 3.10 Reviewer POV is the operational lens

Freeman's slide arc ends at "reviewer POV." Lipton positions reviewers as the
explicit audience. ICML Review Form and ICML Best-Practices are formal
operationalizations. The implication: every writing decision is in service of
making the reviewer's job easy. The reviewer-centric framing is upstream of
the reader-centric framing for *publication* purposes (and reader-centric
framing is upstream for *citation* purposes per [[How-To-Write-1]] §1.3).

---

## 4. Wiki State Post-Batch

- **New subdirectory:** `wiki/writing/` (14 pages).
- **Total wiki pages:** 50 → 64.
- **Index updated:** `## Writing Craft (14)` section added at end of
  `wiki/index.md` under the Agents section. Header count updated 50 → 64.
- **Log entry:** one batch entry appended to `wiki/log.md` (separate operation
  from this report).
- **No `_inbox` paths cited** in any new page's `sources:` field — clean
  contract preserved.
- **No `hard_constraint_possible: unknown`** — all uniformly `no` (writing
  guidance is intrinsically soft).
- **Wikilink hygiene:** all 14 pages use hyphenated-filename form per
  CLAUDE.md Wikilink Convention. One forward reference fix applied during
  lint (`[[Pak-21st-Century-Math-Writing]]` → `[[How-To-Write-1]]` in the
  Strunk-White connections list).

---

## 5. Operational Recommendations (for Bilal)

1. **Pre-submission audit pipeline:** for [[CTPC-KDD-Submission]], consume
   the union of [[ICML-2022-Review-Form]] + [[Ramdas-Paper-Checklist]] +
   [[ICML-Best-Practices]] as a single operational gate. Run the Ramdas 21
   yes/no questions first (cheapest), then verify ICML Best-Practices items
   that haven't already been satisfied, then check the ICML-2022 Review Form
   axes ad hoc as the draft stabilizes.
2. **Writing-agent corpus is now complete enough to wire** — could create a
   `wiki/agents/Writing-Agent.md` definition with these 14 pages as
   `corpus_wiki_pages`. Status would be `ready`. Bilal can request this
   activation as a follow-up.
3. **Open follow-on reads:** Krantz Ch 3-6, Pak §3.3+, Goldreich §4.3-4.6,
   Strunk-White Ch IV-VI. None blocking for KDD '26; revisit during final
   revision pass.
4. **Zinsser's *On Writing Well*** is Pak's primary nonfiction-writing
   recommendation. Not in SiS corpus. Worth a future `raw/writing/`
   expansion if Bilal wants additional ML-paper-writing depth.

---

## 6. Tally of Verbatim Capture vs Paraphrase

Per Bilal's instruction "verbatim capture is more useful than paraphrase" for
checklists and rubrics, the following appear verbatim across the corpus:

- [[ICML-2022-Review-Form]]: all 6 evaluation axes + Phase-1/Phase-2 logic +
  2 declarations verbatim.
- [[Ramdas-Paper-Checklist]]: all 14 bullets verbatim (compressed into 21
  yes/no items separately).
- [[ICML-Best-Practices]]: all 11 sections × ~40 sub-items verbatim.
- [[Strunk-White-Elements-Style]]: all 18 rule titles verbatim.
- [[Ten-Rules-Mathematical-Writing]]: all 10 rules + 2-3-4 rule verbatim.
- [[Milne-Tips-Authors]]: all 11 tip titles + 3 closing quotes verbatim.
- [[Lipton-Writing-Heuristics]]: 17 named heuristics verbatim.

Paraphrase used for: prose-level commentary around the verbatim quotes,
ML-paper-specific calibration sections, SiS-CTPC connection sections.

---

**Report end.**
