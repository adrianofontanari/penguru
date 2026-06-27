# SPEC — `/penguru:snapshot <topic>`

> This is the build contract for the skill. Implement it as `skills/snapshot/SKILL.md`.
> Read `../../CLAUDE.md` first for the principles. This file is the *what*; the principles are the *how well*.

## Goal

One word/phrase in → a **trusted, broad-shallow orientation** the user can absorb in ~3 minutes. Covers every dimension of a topic at depth ★ (light). Two artefacts: a Marp **deck** (skim) and a **reading doc** (reference). Web-only, no MCP, no paid APIs. Every claim sourced or marked "not found".

`<topic>` may be a concept (`AML scoring`, `ePRO`) **or a concrete product idea** (`golf`, `ergonomic chair`, `wifi pill dispenser`).

---

## Step 0 — Ontological triage

Classify the input, then weight nodes and pick authoritative sources accordingly.

| Type | Examples | Emphasized | Light-touch |
|---|---|---|---|
| Technology/Tool | LLM, ePRO, blockchain | N1, N6, N5a, N12 | N9 |
| Methodology/Practice | Agile, DCT, underwriting | N2, N3, N7 | N6 |
| Market/Sector | BNPL, eCOA market | N5a, N10, N9 | N6, N7 |
| Regulatory framework | GDPR, FDA PRO guidance | N7, N1, N2 | N5a |
| Scientific concept | probability of default, risk score | N4, N1, N8 | N5a, N9 |
| **Product/Idea** *(new)* | golf, ergonomic chair, wifi pill dispenser | N1, **Patents**, **Startups**, N3, N5a, N11, N12 | N7, N4 |

Also derive: domain authorities (the "FDA" of this field), 3–5 synonyms/variants, and whether the topic is consumer-tangible (→ product mode).

---

## The 12 nodes — LIGHT (★, 2–3 lines each)

Reuse the deep skills' logic but stop early. Each node: a tiny table or 2–3 bullets, every row sourced.

- **N1 Definitions** — 1–2 authoritative definitions (quoted, sourced) + a minimal glossary. (Logic from `n1-definitions`, trimmed.)
- **N2 Domain** — the wider system it lives in; where it shows up; who uses it.
- **N3 Alternatives** — what came before / competing approaches (for products: competing products).
- **N4 Literature (light)** — 2–3 seminal/recent papers + consensus level (strong/contested/emerging). WebSearch only — no Scopus. Apply the epistemic rule (below) to any number.
- **N5a Market & players** — 4–6 players: who, what they do, positioning. Funding from public web only (press, TechCrunch) — no Crunchbase API. Tag vendor sources.
- **N6 Technical** — how it works, in broad strokes; main components.
- **N7 Regulatory** — key rules/standards: body | document | year | what it requires | link.
- **N8 Adjacent concepts** — what it's confused with, and the distinction.
- **N9 History** — 3–4 milestones + a disruption event + any notable failure.
- **N10 Industry** — analyst firms, associations, key conferences.
- **N11 KOLs** — 2–4 people to follow: name | role | one-line POV | link. POV is opinion → tag 💬.
- **N12 Artefacts** — real things to touch: demo, instrument, playground, PDF, with direct URLs.

### Product/Idea mode — two extra light scans

Activated when Step 0 classifies the input as Product/Idea.

- **Patent scan (web):** Google Patents (`patents.google.com`), Espacenet. Answer: do patents exist? key assignees? is there white space? Output: `patent | assignee | year | what it claims | link`. Feeds the "Market & players" slide as an IP sub-block.
- **Startup scan (web):** Y Combinator companies, Product Hunt, Crunchbase web pages. Answer: who is building this + public funding. Folds into N5a.

In product mode the "Market & players" slide becomes **"Who already makes it + IP"** (products/startups + patent landscape), N3 = alternative products, and the deck explicitly flags the **gap / white space**.

---

## Generation flow

1. Step 0 triage.
2. Run the active nodes in parallel (batch WebSearch; one WebFetch per source worth quoting). Stay light — stop at the first 1–2 trustworthy sources per node.
3. Write **`Reading.md`** (full light detail, all sources).
4. Distil **`Snapshot.md`** (the deck) from `Reading.md`.
5. Confirm to the user (type detected, sources count, file paths, biggest gap).

Both files saved to `./penguru-research/<topic>/`.

---

## Output A — `Snapshot.md` (Marp deck, ~11 slides, 3-min skim)

Marp markdown: frontmatter `marp: true` + a shared theme (see below); slides separated by `---`. Hard cap ~75 words/slide. Tables/bullets only.

1. **Cover** — topic · date · ontological type · one-line "what it is" · source count · trust legend
2. **What it is** — N1 definition (quoted) + glossary mini · N8 don't-confuse-with · N3 before/alternatives
3. **Where it lives** — N2 ecosystem/where it appears · N6 how it works (broad)
4. **Rules** — N7: rule | year | what it requires | link
5. **Market & players** — N5a player | what they do | positioning | source-flag · public funding · N10 analysts/associations · *(Product/Idea → patents + startups + white-space)*
6. **How we got here** — N9: 3–4 milestones + disruption + notable failure
7. **Who to follow** — N11: name | role | one-line POV | link · 💬
8. **What to touch** — N12: demo/instrument/PDF/playground + URL
9. **What the research says** — N4 light: 2–3 papers + consensus
10. **Go deeper** — 3 live threads + ready-made queries for Undermind / Elicit / Perplexity + `/teach` + `/quiz` pointers
11. **Sources & trust audit** — all sources typed; count per type; gaps marked "not found"

## Output B — `Reading.md` (reference document)

Same node order, fuller-but-still-light treatment: the quotes, a few more rows, the context the deck couldn't fit, and the complete source list. This is the document the user returns to. Compressed essence, not an essay.

## Marp theme (shared component)

Ship one theme so every snapshot looks like one product: clean, high-contrast, readable, Tufte-spirited (generous whitespace, restrained color, legible tables). Store it as a reusable asset (e.g. `skills/snapshot/assets/penguru.css`) and reference it from the deck frontmatter. Do not inline per-deck styling.

---

## Trust mechanism (the value prop)

- **Source-type tags** on every claim: ✅ regulator/official · ◽ independent practitioner · ⚠️ vendor · 💬 KOL opinion (not fact).
- **"Not found"** is a valid, required output when no trustworthy source exists. Never fabricate.
- **Epistemic rule** (from `n4-literature`) on every number/threshold/"standard": classify as ✅ regulatory mandate · ⚠️ empirical finding cited out of context · ❌ convention by inertia (no traceable primary source). Trace or flag `[primary source not verified]`.
- **One primary source per dimension** highlighted.
- Final **trust-audit slide**: source count by type + explicit gap list.

## Brevity caps

3-min deck ≈ ~700 words total. ~75/slide. If a node yields more, it goes to `Reading.md`, not the deck.

---

## Deliverables & acceptance

- `skills/snapshot/SKILL.md` implementing the above; `skills/snapshot/assets/` theme.
- Register `snapshot` + `n4-literature` in `.claude-plugin/plugin.json`.
- Fix `[vault-path]` → `./penguru-research/<topic>/` in N1/N4 skills.
- At least one real example: `examples/aml-scoring/Snapshot.md` + `Reading.md`. Add a product example too (e.g. `examples/wifi-pill-dispenser/`).

**Acceptance test:**
1. `/penguru:snapshot AML scoring` → deck renders in Marp, ~11 slides, ~3-min read, every claim sourced, tags present, "not found" where due, trust-audit correct. Spot-check 3 URLs resolve (no hallucinated sources).
2. `/penguru:snapshot wifi pill dispenser` → Product/Idea detected; patent + startup scans present; white-space flagged.
3. `/penguru:snapshot ePRO` → cross-domain triage works (clinical authorities, not fintech).
