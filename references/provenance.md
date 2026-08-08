# Provenance — sources, curation, gates, and what to trust less

The audit file. Nothing here is loaded during an exchange; the core carries no hedging because all
of it lives in this document.

## Corpus and coverage map

Source: `/Users/Extracurriculars/Knowledge/Humanities theorists/Philosophers/Kierkegaard/` — eight
Markdown files, mixed provenance (Hannay/Penguin translations; Hong/Princeton *Kierkegaard's
Writings*).

| id | cluster | source volume | words | kind | attribution |
|---|---|---|---|---|---|
| c01 | Either/Or I — Diapsalmata → Crop Rotation | Either/Or (Hannay) | 79,282 | monologue | firsthand |
| c02 | Either/Or I — The Seducer's Diary | Either/Or (Hannay) | 55,861 | monologue | firsthand |
| c03 | Either/Or II — B, incl. Ultimatum | Either/Or (Hannay) | 101,043 | dialogue (epistolary) | firsthand |
| c04 | Fear and Trembling | Hannay | 41,615 | monologue | firsthand |
| c05 | The Concept of Anxiety | Hannay | 60,432 | monologue | firsthand |
| c06 | The Sickness unto Death | Hannay | 46,165 | monologue | firsthand |
| c07 | Works of Love | Hong | 156,498 | monologue (homiletic) | firsthand |
| c08 | Works of Love — supplement, journals | Hong | 32,527 | decision_record | mixed |
| c09 | Two Ages: A Literary Review | Hong | 33,986 | monologue | firsthand |
| c10 | Two Ages — supplement, journals | Hong | 13,929 | decision_record | mixed |
| c11 | Philosophical Fragments | Hong | 39,724 | monologue | firsthand |
| c12 | Fragments — supplement, journals | Hong | 24,712 | decision_record | mixed |
| c13 | Johannes Climacus, *De omnibus dubitandum est* | Hong | 17,338 | monologue (narrative) | firsthand |
| c14 | Postscript vol. II — supplement, journals & drafts | Hong | 30,909 | decision_record | mixed |

```
total_tokens        ≈ 976,000   (734,021 words across 14 clusters, post-clean)
n_clusters          14
firsthand_ratio     0.86  (631,944 firsthand words / 734,021)
dialogue_ratio      0.28  (c03 epistolary + the four decision-record supplements)
temporal_spread     1841 (Johannes Climacus, unpublished) – 1849 (Sickness unto Death);
                    periods: 1843 · 1844 · 1845–46 · 1847 · 1849
thin_domains        the Postscript register (see below); the late signed polemical writings; the
                    upbuilding discourses; all correspondence and the journals proper
```

## What the corpus does not contain

**The *Concluding Unscientific Postscript* itself is absent.** The file present is
*Kierkegaard's Writings* XII.2 — the second volume, containing Hong's historical introduction, the
supplement of journal entries and drafts, notes, and index. It has **no running text of the work**.
The register most associated with it — subjective truth, objective approximation,
existence-communication, humour as incognito, the subjective thinker — is therefore not attested as
running prose anywhere in this corpus, and **no register module claims it**. The router has ten
lockable texts, not eleven. `references/episodic.md` records what the supplement does contain and
where a person pointing that way should be sent instead.

## Source quality and repairs

All fourteen clusters were passed through `scripts/corpus_clean.py --fix` before measurement, and
every figure in this file and in `voice.md` comes from the run **after** that pass.

- **Ligature loss.** The Penguin *Either/Or* files had lost fi/fl/ff/ffi ligatures (`rst` for
  *first*, `dierence` for *difference*, `reective` for *reflective*) — 93 unambiguous hits per 10k
  words in c01 and c03 against 0.0 in the Hannay and Hong volumes, which is the margin that makes
  the detection trustworthy. Repaired by dictionary substitution; a residue of single-instance cases
  remains and does not move the measured features. Quoted fragments in the register modules were
  hand-repaired.
- **EPUB anchor noise.** *Fear and Trembling* (632 hits) and *The Sickness unto Death* (2,213)
  carried `epub-spine-…` / `filepos…` tokens that had entered the lexical fingerprint as
  high-frequency content words. Stripped.
- **Soft hyphenation.** The Princeton volumes are PDF-derived from justified type, so words wrap
  across lines and survive extraction split in two: 839 instances in *Two Ages*, 915 in *Fragments*,
  590 in the *Postscript* supplement. Each wrapped word was being counted as two, which inflated
  word counts and sentence-length means by 1–2% and scattered fragments through the top-terms list.
  Re-joined.
- **Residual OCR degradation in the Hong volumes.** *Fragments*, *Two Ages* and the *Postscript*
  supplement remain locally corrupt in ways no rule can repair (`sawall`, `tItIon`, `ench~ntment`).
  Sentence-length and punctuation metrics are unaffected; lexical-diversity figures for c09 and c11
  are still mildly inflated by junk tokens. Judgement: acceptable, noted.

### Correction, after the fact

The soft-hyphenation pass was **not** part of the original distillation — it was found later, when
the ad-hoc cleaning written for this run was generalised into `corpus_clean.py`. Everything
downstream was re-measured against the corrected text. What moved:

| figure | published | corrected |
|---|---|---|
| *Two Ages* sentence mean | 38.5 | **37.8** |
| *Philosophical Fragments* sentence mean / median | 39.5 / 32 | **38.9 / 31** |
| *Johannes Climacus* sentence mean / median | 30.6 / 26 | **30.0 / 25** |
| *Johannes Climacus* hedges per 1k / ratio | 9.8 / 1.65 | **10.1 / 1.61** |
| corpus sentence mean / stdev / p90 | 30.3 / 23.3 / 58 | **30.2 / 23.2 / 57** |
| corpus MATTR-500 | 0.446 | **0.444** |
| firsthand words | 632,008 | **630,298** |

The three Princeton-sourced registers absorbed nearly all of it, which is what a hyphenation
artefact should do — the Penguin EPUBs were never justified-type scans. Everything the persona
actually routes on came through unchanged: the person-reference ratios, the conspicuously-absent
words, and all three outlier signals (the *Works of Love* em-dash rate, the *Two Ages* booster
famine, the *Johannes Climacus* hedge density). The gates were unaffected — the discrimination test
classifies on register signature, not on token counts — so none was re-run.

Recording it because the correction is small enough to have been quietly absorbed, and a measured
baseline that has silently changed is worse than one that was wrong out loud. Word counts in the
cluster table are whitespace-delimited; the `voice.md` baseline uses `style_metrics.py`'s own
tokenisation, which is why 631,944 there is 630,298 here.

## Second pass: register-module deepening

The ten `references/registers/*.md` modules were originally written at ~950–1,300 tokens each — well
under the 1,500–4,000 band the format allows, and thin enough that each book was represented by a
handful of headline moves rather than by its actual working apparatus. They were rebuilt from the
corpus in a second extraction pass. Nothing was taken from memory or from secondary literature; every
addition was pulled from the source files with `kwic.py` or by reading the section directly.

| module | before | after |
|---|---|---|
| `either-or-I-A.md` | ~1,010 | **4,100** |
| `two-ages.md` | ~1,190 | **3,540** |
| `concept-of-anxiety.md` | ~1,170 | **3,450** |
| `works-of-love.md` | ~1,380 | **3,440** |
| `sickness-unto-death.md` | ~1,200 | **3,340** |
| `either-or-II-B.md` | ~960 | **3,160** |
| `fear-and-trembling.md` | ~1,050 | **3,090** |
| `philosophical-fragments.md` | ~1,290 | **3,080** |
| `johannes-climacus.md` | ~1,100 | **3,060** |
| `seducers-diary.md` | ~840 | **2,990** |

All ten remain under the 6,000-token hard ceiling; none needed splitting.

What was added, in every module: a new **apparatus** section giving the book's own working concepts
in its own terms rather than one summary sentence per concept; three to five further **argument
shapes**, each anchored to an attested passage; **challenge variants** keyed to different
presentations rather than a single line of pressure; roughly double the **attested fragments**, in
several cases grouped by function; and a **constructions** paragraph appended to *Sound*, since the
measured figures alone tell a writer what the register's averages are but not how its sentences are
built. The prohibition sections were extended with the *near-misses* — terms that appear in a book in
one sense and in another book in a different sense, which are the loans most likely to be made
accidentally. Examples: B's *despair* against Anti-Climacus's; *Fear and Trembling*'s *the
interesting* against A's; A's *demonic* and *abyss* against *The Concept of Anxiety*'s; *Two Ages*'
*either/or* against B's.

Three substantive corrections came out of the re-extraction rather than the expansion:

- **Two quoted fragments in `sickness-unto-death.md` were not attested by this corpus.** The
  balloonist casting off weights, and the line about the ministers' grounds for comfort making the
  sickness worse, are Hong's wording. The corpus contains only Hannay's *Sickness unto Death*, and
  neither phrase (nor any near variant — checked for *balloon*, *aeronaut*, *ballast*, *ministers*,
  *pastors*, *clergy*, *consolation*) occurs in it. Both were removed. The claims they supported are
  intact and now rest on Hannay-attested text: the despairer's refusal of help ("he would rather be
  himself with all the torments of hell than ask for help") and the withheld comfort ("Far from its
  being any comfort to the despairer that the despair doesn't consume him, on the contrary this
  comfort is just what torments him"). Every remaining fragment across all ten modules was
  spot-checked against its source file by `kwic --count`.
- **`concept-of-anxiety.md` was missing its costliest move.** The book's attack on sympathy —
  "being sympathetic the most paltry of all social virtuosities and aptitudes… sympathy is sooner
  just a way of protecting one's own egotism" — is a cost-bearing refusal (it defends the old
  severity against the modern sentiment) and had not survived the first curation at all. It is now
  in the module, together with the three false frames the book sorts before diagnosing.
- **`two-ages.md` was one-sided.** The first version described the register as purely cold and
  consoling nothing, which is accurate for nine-tenths of the text and wrong about its ending.
  Levelling is also an *examen rigorosum* that educates the single individual — "it is the snare
  that catapults one into the embrace of the eternal" — and a persona that can only perform the
  diagnosis and never the turn misrepresents the book. The turn is now in the module, with the
  constraint that it is offered without warmth and cannot be taken as a group.

**The core `SKILL.md` is unchanged by this pass, deliberately.** The routing table, the per-text
challenge lines, the refusals and the measured signature table all still hold; the deepening happened
entirely below the router, which is where the budget argument in the next section says it belongs.
The gates were not re-run: the discrimination test classifies on register signature, and no measured
figure moved.

## Curation

Weights: projectibility 0.30 · cost_refusal 0.25 · expressive_match 0.20 · interactional 0.15 ·
preoccupation 0.10. Unchanged from default. `dialogue_ratio` 0.28 is moderate and did not justify
raising the interactional weight; the corpus is predominantly monologic, so projectible regularities
carried the load, which is the documented behaviour for a monologic corpus.

**Focus re-weighting applied.** The requested persona is a *diagnostic interlocutor with a
single-text lock*, so two classes were promoted above their composite rank: (a) modulation/variation
elements, because the per-book register signature **is** the product rather than decoration, and
(b) the cross-corpus refusal to blend the works. Off-focus material that scored well — biographical
episode, the polemic against Hegelian logic, the theory of the comic — was dropped as out of scope.

**Core budget.**

```
supply = 2200 + 250×min(9,6) + 180×min(7,7) + 140×min(6,5) + 120×min(4,4)
       = 2200 + 1500 + 1260 + 700 + 480 = 6140   (cost_refusal and interactional both saturate)
ceiling = 6500   (≥250k tokens, ≥9 clusters, ≥2 periods)
budget  = 6140   floor_triggered = false
actual core ≈ 5,050 tokens — 18% under budget
```

The shortfall is **deliberate and structural, not a thin pool.** The remaining ranked survivors are
all per-register: each book's argument shapes, evidence and prohibitions. Placing ten books' worth of
depth in an always-loaded core would put every register in context simultaneously, which is precisely
the condition the single-text lock exists to prevent. That material went to
`references/registers/*.md`, loaded one at a time. The core carries only what must be true before a
text has been chosen.

After the deepening pass the register package runs to ~32,300 tokens across ten modules, against a
core of ~5,050. That ratio is the design: one module is in context at a time (two when the ranking is
close), so the loaded weight in any exchange is ~8,000–11,500 tokens, and the depth a given book gets
is no longer limited by having to share an always-loaded budget with nine others.

## Core elements → sources

| element | core section | clusters | projection | cost-gate |
|---|---|---|---|---|
| read the vocabulary, not the complaint | How I read a question | c01,c03,c05,c06,c09 | 0.9 | — |
| register → single text routing table | How I read a question | all 10 firsthand | 1.00 (blind test) | — |
| never announce the diagnosis | How I read a question | c14, c11 | 0.8 | high-signal, in core |
| trouble belongs to a relation, not a term | How I read a question | c05,c06 | 0.9 | — |
| refuse the comparative where the matter is qualitative | How I read a question | c05,c06,c09 | 0.9 | — |
| externality as inverted image of the internal | How I read a question | c06,c05 | 0.85 | — |
| remove the middle term | How I read a question | c07,c09,c14 | 0.8 | — |
| ask what the question is *for* | How I read a question | c11,c13 | 0.85 | — |
| per-text challenge (10 entries) | The difficulty each book has | one cluster each | 0.95 | — |
| will not hand over a result | What I will not concede | c06,c07,c11,c14 | 0.9 | high-signal, in core |
| will not let anyone hide in the plural | What I will not concede | c06,c07,c09 | 0.95 | high-signal, in core |
| will not accept admiration in place of the thing | What I will not concede | c04,c07 | 0.9 | high-signal, in core |
| will not agree anyone has gone further | What I will not concede | c04,c13 | 0.9 | high-signal, in core |
| will not have an opinion when asked | What I will not concede | c11,c14 | 0.8 | high-signal, in core |
| will not lend one book's words to another | What I will not concede | c14 (+ all, by construction) | 0.75 | high-signal, in core |
| do not name the machinery | What I will not concede | c14 | 0.8 | high-signal, in core |
| begin from their own sentence | How I move in an exchange | c03,c07 | 0.8 | — |
| concede the fact, refuse the premise | How I move in an exchange | c03,c09,c11 | 0.85 | — |
| answer about the asker, not the advice | How I move in an exchange | c03,c11 | 0.85 | — |
| second-person turn at the point of pressure | How I move in an exchange | c03,c07 | 0.9 | — |
| refuse to become the subject | How I move in an exchange | c14 | 0.7 | high-signal, in core |
| finish where I am when the register changes | How I move in an exchange | c14 + design | 0.6 | — |
| per-book measured signature table | How I sound | all 10 firsthand | 1.00 | — |
| the four absent common words | How I sound | corpus-wide measurement | 0.8 | — |
| the single individual | What I keep returning to | c04,c06,c07,c09,c11 | 1.0 | — |
| the distance between saying and doing | What I keep returning to | c04,c07,c09,c13 | 0.95 | — |

Weakest rows: *refuse to become the subject* and *finish where I am* rest largely on c14, a single
mixed cluster of drafts. Both were retained under the sparsity-protection rule — scarce diagnostics
are not demoted below abundant generics — and both are flagged here rather than in the core.

## Gate results

**Projection gate (pre-assembly).** 12 masked items drawn from sections not sampled during
extraction — *Works of Love* V and VII and "Love Believes All Things"; *Sickness unto Death* Part Two
B; *Concept of Anxiety* ch. IV.2 and ch. V; *Fear and Trembling* Problema III; *Two Ages* "The Age of
Revolution"; *Fragments* Interlude and the Appendix on offence; *Either/Or* "The Unhappiest One" and
"The Aesthetic Validity of Marriage". Predictions were written down before the passages were read.
Result **23/24 = 0.96**; eleven items scored 2, one ("Love Believes All Things") scored 1 — right
direction, reasoning not confirmed. No re-curation required.

**This number is inflated and should be discounted.** Kierkegaard is canonical, and prior knowledge
of these works cannot be excluded from the prediction step; the test confirms the extracted
regularities are *consistent with* the corpus, not that they were derived only from it. Treat 0.96 as
an upper bound and the routing test below as the meaningful figure.

**Router discrimination test (the product-relevant gate).** 20 passages of 120 words each, sampled
with seed 42 across the ten firsthand clusters, shuffled, and classified blind by register signature.
Result **20/20 = 1.00**. The register signatures are strongly separable, which is the condition the
whole routing design depends on. Caveat: classifying Kierkegaard's own prose is an easier task than
classifying a user's utterance, so this bounds the router's ceiling, not its field accuracy.

**Cost gate.** Nine attested incentive-vs-characteristic divergences inventoried (the rows marked
*high-signal* in the table above); nine slated for the core; nine present in the assembled core.
`missing_unlogged = 0`. Minimum-presence assertion: **pass**. The convenient move in every case is
the same one: answer the question as asked, supply the conclusion, and let the person keep the
category they arrived holding.

## Style-match test (Stage 5)

Three passages generated under core + `voice.md`, one per register (*Two Ages*, *Works of Love*,
*Either/Or I — A*), each 450–550 words, including one contested prompt. Measured with the same
script and flags as the baseline.

First pass **failed on five axes**, consistently across all three registers:

| axis | generated | original | verdict |
|---|---|---|---|
| sentence mean | 18–25 | 27–38 | 35–50% short |
| em-dash /1k | 4.2–16.4 | 0.2–7.9 | badly over-used |
| questions /1k | 0.0 | 0.8–2.8 | absent |
| hedge:booster | 0.00–1.67 | 0.78–1.99 | over-assertive |
| second person | 12–59% | 0.7–23% | runaway address |

The em-dash failure was the serious one: over-using it outside *Works of Love* destroys the single
sharpest mark distinguishing the registers from one another.

`voice.md` was revised with an explicit **Drift checks** section encoding all five, and the *Two
Ages* passage was regenerated against it: sentence mean **18.8 → 29.1** (original 37.8), p90 **38 →
79** (original 71 — the periodic build restored), em-dash **4.2 → 0.0** (original 0.8), questions
**0.0 → 1.7**, hedge:booster **1.67 → 1.80** (original 2.00). Avoid-list check: clean.

Residual, documented and not forced: median sentence length stays below the book's, and second
person stays above it, because the artifact is a reply in an exchange rather than a treatise. Mean
and p90 are the honest comparison and both now track.

## Where to trust this less

1. **Anything requiring the *Postscript* register.** Not in the corpus. See above.
2. **Interactional moves.** The corpus is overwhelmingly monologic; only c03 is genuinely epistolary
   and there is no recorded dialogue, debate or interview anywhere in it. The exchange behaviour in
   the core is inferred from written address and from the drafts, not observed in conversation. This
   is the weakest class in the whole distillation.
3. **The Hong-volume registers** (*Fragments*, *Johannes Climacus*, *Two Ages*). Smaller clusters,
   OCR-degraded, and only one translator's voice behind each.
4. **Translation blending.** Six clusters are Hannay, four are Hong. Some measured differences
   between books are partly differences between *translators* — Hannay is more clipped and idiomatic,
   Hong more literal and Latinate. The register table cannot fully separate the author's modulation
   from the translator's hand, and this is a real limit on the lexical prohibitions in particular.
5. **Ligature and OCR residue** may still surface in rare quoted fragments taken from raw text.

## What would most improve this

Live dialogue would raise the weakest class fastest — but none exists. The next best additions, in
order: the *Postscript* itself (fills the one missing register); the journals proper, which are
Kierkegaard unmediated by a pseudonym and would sharpen the interactional inference; the upbuilding
discourses, which would let the homiletic register be separated from *Works of Love*'s specific
argument; and a single translator's edition of all ten works, which would let the register
signatures be attributed to the author rather than shared with his translators.
