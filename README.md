# penguru

![penguru](penguru-icon.png)

> A trusted snapshot research agent. One word in → a 3-minute, source-cited orientation on anything.

You know nothing about a topic and need a footing — fast and trustworthy. LLMs hallucinate sources, flatten nuance, and can't tell a regulator from a vendor. penguru is **one one-shot skill** that gives you the first three minutes on any topic: broad across every relevant dimension — definitions, market, regulation, history, key people, patents — shallow per dimension, every claim sourced or marked *not found*.

It stands on its own. No referrals to other platforms — it ends by suggesting adjacent topics you can snapshot next.

Topics can be abstract (`AML scoring`, `ePRO`) or concrete product ideas (`ergonomic chair`, `wifi pill dispenser`) — products also get a patent and startup scan.

## The skill

### `/penguru:snapshot <topic>`

A broad-shallow pass over every relevant dimension, output as two artefacts in `./penguru-research/<topic>/`:
- **`Snapshot.md`** — a Marp slide deck, a 3-minute skim
- **`Reading.md`** — a reference document with the fuller detail

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
/penguru:snapshot ePRO
```

## Background

Built on a faceted knowledge model — DIKW (Ackoff), Ranganathan's faceted classification, Information Foraging Theory (Pirolli & Card) — and learning-science principles: cite everything, respect working memory, reference documents over essays.

Earlier per-dimension skills (`n1-definitions`, `n4-literature`) are archived under [`archived/`](archived/).

## Credits

Icon by OpenAI ChatGPT. Plugin built with [Claude Code](https://claude.ai/code).
