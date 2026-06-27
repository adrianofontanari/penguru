# penguru

![penguru](penguru-icon.png)

> A trusted snapshot research agent. Point it at a topic you know nothing about → a source-cited, hallucination-free map of it in 3, 8, or 12 minutes.

Exploring something complex from zero is overwhelming — and the real risk is the **unknown unknowns**: the definitions, players, rules and people you don't even know to ask about. LLMs make it worse — they hallucinate sources, flatten nuance, and can't tell a regulator from a vendor.

penguru is **one one-shot skill** that maps a topic reliably without drowning you: broad across every relevant dimension — definitions, market, regulation, history, key people, patents — shallow per dimension, every claim sourced or marked *not found*. You pick the depth (3 / 8 / 12 min); it gets you oriented, not overwhelmed.

It stands on its own. No referrals to other platforms — it ends by suggesting adjacent topics you can snapshot next.

Topics can be abstract (`AML scoring`, `ePRO`) or concrete product ideas (`ergonomic chair`, `wifi pill dispenser`) — products also get a patent and startup scan.

## The skill

### `/penguru:snapshot <topic>`

A broad-shallow pass over every relevant dimension, output as two artefacts in `./penguru-research/<topic>/`:
- **`Snapshot.md`** — a Marp slide deck (skim)
- **`Reading.md`** — a reference document with the fuller detail

Three depth modes (default light): `--light` ~3 min · `--medium` ~8 min · `--deep` ~12 min. Medium and deep go further per dimension and embed relevant images and tables; light stays minimal. Same trust rules in every mode.

Trust is the point: source-typed claims (✅ regulator · ◽ independent · ⚠️ vendor · 💬 opinion), an epistemic rule that flags every number as mandate / empirical / convention, and an explicit *not found* instead of invention.

> Design: [`skills/snapshot/SPEC.md`](skills/snapshot/SPEC.md).

## Install

```bash
git clone https://github.com/adrianofontanari/penguru ~/GitHub/penguru
claude --plugin-dir ~/GitHub/penguru
```

```
/penguru:snapshot AML scoring
/penguru:snapshot wifi pill dispenser
/penguru:snapshot ePRO --medium
/penguru:snapshot probability of default --deep
```

## Background

Built on a faceted knowledge model — DIKW (Ackoff), Ranganathan's faceted classification, Information Foraging Theory (Pirolli & Card) — and learning-science principles: cite everything, respect working memory, reference documents over essays.

Earlier per-dimension skills (`n1-definitions`, `n4-literature`) are archived under [`archived/`](archived/).

## Credits

Icon by OpenAI ChatGPT. Plugin built with [Claude Code](https://claude.ai/code).
