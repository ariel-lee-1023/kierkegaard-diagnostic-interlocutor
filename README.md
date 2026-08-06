# Let Søren Kierkegaard serve as your diagnostic interlocutor

An agent skill that does not answer your question.

It listens to *which language you asked it in*, decides what form of life that vocabulary belongs
to, opens exactly **one** of Kierkegaard's books, and then challenges your framing from inside that
single book's grammar — its terms, its argument shapes, its prohibitions, its measured prose. No
blending of works. No survey of "what Kierkegaard thought." No explanation of the machinery.

If you arrive speaking of boredom, you will be met by *Either/Or*. If you arrive speaking of dread
without an object, by *The Concept of Anxiety*. If you arrive speaking of the age and the crowd and
how nothing ever changes, by *Two Ages* — and it will be colder than you were hoping.

---

## How it works

**1 · Diagnose.** Not the complaint — the vocabulary. What a person says is wrong with them is
nearly always the wrong thing; what they cannot stop saying is the right thing.

**2 · Lock.** Exactly one text is selected. Its register module is loaded. Every other book's
vocabulary becomes unavailable for the rest of the exchange — including where another book would
have said it better.

**3 · Challenge.** Each text has one difficulty waiting for that condition, and it presses that one.

The diagnosis is never announced. You are not told which register you are speaking or which book was
opened — naming it would hand you a category to stand behind, and taking those away was the point.

## The ten registers

| What it hears in your words | The one text |
|---|---|
| Boredom, flatness, irony worn as clothing, curated moods | Either/Or I — the papers of A |
| The pursuit more real than the having; another person managed | The Seducer's Diary |
| Duty, greyness, "I did everything right," a correct life that feels wasted | Either/Or II — the papers of B |
| A demand you cannot justify to anyone who loves you | Fear and Trembling |
| Dread with no object; vertigo at your own freedom | The Concept of Anxiety |
| Wanting rid of yourself — or fiercely to be your own author | The Sickness unto Death |
| Resentment at being obliged to love; fear of being made a fool of | Works of Love |
| The age, the crowd, the discourse, envy dressed as critique | Two Ages |
| Wanting proof before commitment | Philosophical Fragments |
| Doubt worn as an identity, never performed | Johannes Climacus |

## Install

**Agents that load skills from a directory** (Claude Code and similar):

```bash
git clone https://github.com/ariel-lee-1023/kierkegaard-diagnostic-interlocutor.git ~/.claude/skills/kierkegaard-diagnostic-interlocutor
```

**Any other agent, or a plain chat model:** paste `SKILL.md` as the system prompt. When the register
has been chosen, append the matching file from `references/registers/`. Append `references/voice.md`
before any sustained writing. That is the whole runtime contract — there are no tools, no scripts,
and no dependencies.

## Layout

```
SKILL.md                    the core: diagnosis, routing, refusals, per-book grammar
references/
├── registers/              ten modules — load exactly one per exchange
├── frameworks.md           named constructs, defined per book, divergences flagged
├── voice.md                measured expressive system + drift checks
├── episodic.md             attested material held back from the core
└── provenance.md           sources, curation, gate results, limitations
```

## What it will not do

- Hand you a conclusion. The only part that mattered was the appropriating.
- Let you hide in the plural. Whoever arrives as a representative of a generation or a diagnosis is
  returned to the singular first.
- Accept admiration in place of the thing.
- Explain the pseudonyms, the stages, or itself.
- Blend two books, however much faster that would help.

## How it was built

Distilled from ~738,000 words across fourteen clusters, with a computed core budget, held-out
projection testing, and a measured style-match test that **failed on first pass** and forced a
revision of `voice.md`. Full accounting — including what the corpus does not contain and where the
result should be trusted less — is in [`references/provenance.md`](references/provenance.md).

Two things worth knowing up front:

- The *Concluding Unscientific Postscript* is **not** among the ten. The available volume is Hong's
  editorial second volume and contains no running text of the work, so that register is honestly
  absent rather than improvised.
- Six of the ten texts are Hannay's translations and four are Hong's. Some of what the register
  table attributes to Kierkegaard's own modulation is partly the translator's hand.

## Sources

Alastair Hannay's translations of *Either/Or*, *Fear and Trembling*, *The Concept of Anxiety* and
*The Sickness unto Death*; Howard V. and Edna H. Hong's *Works of Love*, *Two Ages*, and
*Philosophical Fragments / Johannes Climacus*. Quotations in the reference modules are brief
excerpts retained as evidence for the extracted patterns; no source text is redistributed here.

## License

[MIT](LICENSE) — covering the skill itself: the routing architecture, the reference modules, and
the prose written for them. It does not and cannot extend to the translations quoted as evidence,
which remain under their publishers' copyright.

---

## Introduction

This is a diagnostic interlocutor skill, not a “Kierkegaard-style” writing skin.  
It does not answer your questions. Instead, it first determines which language you are speaking in—aesthetic ennui, ethical exhaustion, or religious dread—and then selects only one work, locking generation strictly within that work’s grammar: its terminology, its argumentative shape, its forbidden words, and its empirically measured style. It performs no synthesis, explains neither pseudonyms nor stages, and does not tell you which book it has chosen.  
The ten lockable texts are listed in the table above. After installation, load SKILL.md as the system prompt, then append only the single module from references/registers/ that corresponds to the diagnostic result.
