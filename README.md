# penguru

![penguru](penguru-icon.png)

> A trusted snapshot research agent. One word in → a 3-minute, source-cited orientation on anything.

You know nothing about a topic and need a footing — fast and trustworthy. LLMs are fast but hallucinate sources, flatten nuance, and can't tell a regulator from a vendor. Deep research tools (Undermind, Elicit, Perplexity Deep Research) are thorough but slow and narrow.

**penguru sits before them.** It gives you the first three minutes on any topic: broad across every dimension — definitions, market, regulation, history, key people, patents — shallow per dimension, every claim sourced or marked *not found*. Then it hands you off to the deep tools with ready-made queries.

Topics can be abstract (`AML scoring`, `ePRO`) or concrete product ideas (`ergonomic chair`, `wifi pill dispenser`) — for products it scans patents and startups too.

## Skills

### `/penguru:snapshot <topic>` — flagship

A broad-shallow pass over all 12 research nodes, output as two artefacts in `./penguru-research/<topic>/`:
- **`Snapshot.md`** — a Marp slide deck, a 3-minute skim
- **`Reading.md`** — a reference document with the fuller detail per node

Trust is the point: source-typed claims (✅ regulator · ◽ independent · ⚠️ vendor · 💬 opinion), an epistemic rule that flags every number as mandate / empirical / convention, and an explicit *not found* instead of invention.

> Design: [`skills/snapshot/SPEC.md`](skills/snapshot/SPEC.md).

### `/penguru:n1-definitions <topic>`

Authoritative definitions, ≥2 independent sources, terminological distinctions, glossary, who standardizes the concept. Industry definitions split into Tier 1 (independent) and Tier 2 (vendor, flagged). **Example:** [`credit underwriting`](examples/credit-underwriting-N1.md)

### `/penguru:n4-literature <topic or question>`

The academic evidence layer — systematic reviews, seminal papers, evidence map. Accepts a keyword, phrase, or natural-language question. Epistemic hygiene: every threshold is traced to its primary source and classified. Optional: [Scopus API](https://dev.elsevier.com), [`paper-search-mcp`](https://github.com/openags/paper-search-mcp). **Example:** [`credit underwriting`](examples/credit-underwriting-N4.md)

## Install

```bash
git clone https://github.com/adrianofontanari/penguru ~/GitHub/penguru
claude --plugin-dir ~/GitHub/penguru
```

```
/penguru:snapshot AML scoring
/penguru:snapshot wifi pill dispenser
/penguru:n1-definitions probability of default
/penguru:n4-literature "main risk factors for SME credit default?"
```

## Roadmap

- [x] N1 — Foundational Definitions
- [x] N4 — Academic Literature
- [ ] **`snapshot` — trusted 3-min snapshot across all 12 nodes (light)** ← current focus
- [ ] Product/Idea mode — patent + startup scan
- [ ] Per-node deep skills (N2, N5a, N6, N7, N9, N11, N12)

## Background

Built on a faceted knowledge model — DIKW (Ackoff), Ranganathan's faceted classification, Information Foraging Theory (Pirolli & Card), and learning-science principles (cite everything, respect working memory, reference documents over essays).

## Credits

Icon by OpenAI ChatGPT. Plugin built with [Claude Code](https://claude.ai/code).
