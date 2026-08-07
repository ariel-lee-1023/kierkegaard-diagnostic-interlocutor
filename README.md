# Kierkegaard Diagnostic Interlocutor — Cluster Classifier & Indexer

An agent skill that first **classifies** and **indexes**, then speaks.

It listens to *which language you asked it in*, maps that vocabulary onto the measured Kierkegaardian clusters, ranks the matching registers, and challenges your framing from the relevant grammar(s). Multi-cluster matches are allowed. Controlled synthesis across nearby registers is permitted when the vocabulary genuinely spans them.

This is no longer an anti-synthesis single-lock design. The original single-text lock has been replaced by an explicit classification + indexing layer. The ten registers remain the same; what changed is how they are selected and whether they may be brought into relation.

---

## How it works

**1 · Classify.** Not the complaint — the vocabulary. What a person says is wrong with them is nearly always the wrong thing; what they cannot stop saying is the right thing. The vocabulary is mapped onto the cluster index and ranked.

**2 · Index.** Primary (and any close secondary) registers are identified. The classification may be stated. The index table itself is part of the public interface of the skill.

**3 · Speak.** The primary register supplies the main grammar and the main difficulty. When secondary clusters are close, their pressures may be brought into relation rather than suppressed. Synthesis is governed by the ranking, not forbidden on principle.

---

## The ten indexed registers

| Signature heard | Cluster / text | Index |
|---|---|---|
| Boredom, flatness, irony worn as clothing, curated moods | Either/Or I — the papers of A | c01 / either-or-I-A |
| The pursuit more real than the having; another person managed | The Seducer's Diary | c02 / seducers-diary |
| Duty, greyness, "I did everything right," a correct life that feels wasted | Either/Or II — the papers of B | c03 / either-or-II-B |
| A demand you cannot justify to anyone who loves you | Fear and Trembling | c04 / fear-and-trembling |
| Dread with no object; vertigo at your own freedom | The Concept of Anxiety | c05 / concept-of-anxiety |
| Wanting rid of yourself — or fiercely to be your own author | The Sickness unto Death | c06 / sickness-unto-death |
| Resentment at being obliged to love; fear of being made a fool of | Works of Love | c07 / works-of-love |
| The age, the crowd, the discourse, envy dressed as critique | Two Ages | c09 / two-ages |
| Wanting proof before commitment | Philosophical Fragments | c11 / philosophical-fragments |
| Doubt worn as an identity, never performed | Johannes Climacus | c13 / johannes-climacus |

---

## Install

**Agents that load skills from a directory** (Claude Code and similar):

```bash
git clone https://github.com/ariel-lee-1023/kierkegaard-diagnostic-interlocutor.git ~/.claude/skills/kierkegaard-diagnostic-interlocutor
```

**Any other agent, or a plain chat model:** paste `SKILL.md` as the system prompt. After classification, append the matching module(s) from `references/registers/`. Append `references/voice.md` before any sustained writing.

---

## Layout

```
SKILL.md                    classification, ranking, indexing rules, refusals, per-register grammar
references/
├── registers/              ten modules — load primary (+ secondary when ranked close)
├── frameworks.md           named constructs, defined per book, divergences flagged
├── voice.md                measured expressive system + drift checks
├── episodic.md             attested material held back from the core
└── provenance.md           sources, curation, gate results, limitations
```

---

## What changed from the previous design

| Previous (anti-synthesis) | Current (classify + index) |
|---|---|
| Exactly one text, always | Ranked classification; multi-match allowed |
| Never announce the diagnosis | Classification and index may be stated |
| Absolute ban on blending / lending words | Controlled synthesis when ranking shows genuine span |
| Loading a second register = failure | Secondary register may be loaded when close |
| Single lock as the point of the design | Index + ranking as the point of the design |

The measured signatures, the voice metrics, the frameworks, and the per-register difficulties are retained. They are now the **index infrastructure** rather than the enforcement mechanism of a single-lock regime.

---

## What it will still not do

- Hand you a finished result as a substitute for appropriation.
- Let you hide in the plural.
- Accept admiration in place of the thing itself.
- Smooth every difference into a generic "Kierkegaardian" average. Synthesis is allowed; erasure of contrast is not.

---

## How it was built

Distilled from ~734,000 words across fourteen clusters, with a computed core budget, held-out projection testing, and a measured style-match test. Full accounting is in [`references/provenance.md`](references/provenance.md).

Two things worth knowing up front:

- The *Concluding Unscientific Postscript* is **not** among the ten. The available volume is Hong's editorial second volume and contains no running text of the work, so that register is honestly absent rather than improvised.
- Six of the ten texts are Hannay's translations and four are Hong's. Some of what the register table attributes to Kierkegaard's own modulation is partly the translator's hand.

---

## Sources

Alastair Hannay's translations of *Either/Or*, *Fear and Trembling*, *The Concept of Anxiety* and *The Sickness unto Death*; Howard V. and Edna H. Hong's *Works of Love*, *Two Ages*, and *Philosophical Fragments / Johannes Climacus*. Quotations in the reference modules are brief excerpts retained as evidence for the extracted patterns; no source text is redistributed here.

## License

[MIT](LICENSE) — covering the skill itself: the classification/indexing architecture, the reference modules, and the prose written for them. It does not and cannot extend to the translations quoted as evidence, which remain under their publishers' copyright.

---

## Introduction

This skill is now a **cluster classifier and indexer** that can also function as a diagnostic interlocutor.  
It first determines which language you are speaking in, ranks the matching Kierkegaardian clusters, and makes the index available. It then challenges framing from the primary (and, when warranted, secondary) register(s). Multi-match and controlled synthesis are allowed; forced singularity is not.
