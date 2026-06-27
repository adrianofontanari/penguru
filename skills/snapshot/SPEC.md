# SPEC — `/penguru:snapshot <topic>`

> Build contract for the skill. Implement as `skills/snapshot/SKILL.md`.
> Read `../../CLAUDE.md` first for the principles. This file is the *what*.

## Goal

penguru is **one one-shot skill**. Input a topic → output a **trusted, broad-shallow orientation** absorbable in ~3 minutes. Broad across every relevant dimension, shallow per dimension. Web-only, no MCP, no paid APIs. Every claim sourced or marked *not found*. Self-contained — the report does **not** point the user to other research platforms.

`<topic>` may be a concept (`AML scoring`, `ePRO`) **or a concrete product idea** (`golf`, `ergonomic chair`, `wifi pill dispenser`).

Two artefacts in `./penguru-research/<topic>/`:
- **`Snapshot.md`** — a Marp slide deck, a skim
- **`Reading.md`** — a reference document, fuller detail per dimension

## Modes (depth dial)

`/penguru:snapshot <topic> [--light | --medium | --deep]` — **default `--light`** (zero friction). If no flag, light.

| Mode | Read time | Coverage | Sources/dim | Images & tables |
|---|---|---|---|---|
| **light** (default) | ~3 min | core relevant dimensions | 1–2 | tables only when they clarify; images only if genuinely necessary |
| **medium** | ~8 min | all relevant dimensions, fuller | 2–3, corroborated | include relevant tables **and images** (diagrams, charts, product/patent figures) |
| **deep** | ~12 min | all relevant dimensions, deepest one-shot | 3+, cross-checked | rich tables and images throughout |

Mode scales **volume** (sources, rows, slide count, reading-doc length) and image/table inclusion — **not** the trust rules, which are identical in every mode. The read-time target is the budget; overflow trims to the most load-bearing content first.

## Step 0 — Triage

Classify the topic, then weight dimensions and pick authoritative sources accordingly.

| Type | Examples | Emphasize | Light |
|---|---|---|---|
| Technology/Tool | LLM, ePRO | definitions, technical, market, artefacts | history |
| Methodology/Practice | Agile, underwriting | domain, alternatives, regulation | technical |
| Market/Sector | BNPL, eCOA market | market/players, industry, history | technical, regulation |
| Regulatory framework | GDPR, FDA guidance | regulation, definitions, domain | market |
| Scientific concept | probability of default | research, definitions, adjacent | market, history |
| **Product/Idea** | golf, ergonomic chair, wifi pill dispenser | definitions, **patents**, **startups**, alternatives, market, reviews, real products | regulation, research |

Also derive: domain authorities (the field's "FDA"), 3–5 synonyms/variants, whether the topic is consumer-tangible (→ product mode).

**Product mode is auto-detected here — zero friction, the user never activates it.** The skill decides from the topic alone. If the topic is a tangible product/idea, the patent + startup scans run; otherwise they silently don't. No flags, no prompts.

## Dimensions (light — 2–3 lines each, every row sourced)

Forget node numbering — these are the sections of one report. Cover the ones the triage marks relevant; skip the rest gracefully.

- **Definitions & glossary** — 1–2 authoritative definitions (quoted, sourced) + minimal glossary of terms of art.
- **Domain & how it works** — the wider system it lives in, where it shows up, who uses it, and broad-strokes mechanics.
- **Alternatives / before** — what came before / competing approaches (for products: competing products).
- **Market & players** — 4–6 players: who, what they do, positioning. Funding from public web only (press, TechCrunch). Tag vendor sources. Fold in analysts/associations if relevant.
- **Regulation & standards** — body | document | year | what it requires | link.
- **Adjacent concepts** — what it's confused with, and the distinction. *(Also feeds the "Next: explore" section.)*
- **History** — 3–4 milestones + a disruption event + any notable failure.
- **People to follow** — 2–4 KOLs: name | role | one-line POV | link. POV is opinion → tag 💬.
- **Artefacts** — real things to touch: demo, instrument, playground, PDF, with direct URLs.
- **Research snapshot** — 2–3 seminal/recent papers + consensus (strong/contested/emerging). WebSearch only. Apply the epistemic rule (below) to any number.

### Product/Idea mode — two extra light scans

- **Patent scan (web):** Google Patents (`patents.google.com`), Espacenet → do patents exist? key assignees? white space? Output: `patent | assignee | year | claim | link`.
- **Startup scan (web):** Y Combinator, Product Hunt, Crunchbase web → who's building it + public funding.

In product mode "Market & players" becomes **"Who makes it + IP"** (products/startups + patent landscape), Alternatives = competing products, and the report flags the **gap / white space**.

## Next: explore (the one forward-looking section)

The report ends by surfacing **3–5 adjacent concepts** worth a separate snapshot — drawn from Adjacent / Domain / Alternatives. Each: `concept — one line why it's relevant`. The user explores one by running `/penguru:snapshot <that concept>` again (Step 0 re-weights for the new topic). This is the only "next step" — **internal exploration, never a referral to an external platform.**

## Generation flow

1. Triage. 2. Run relevant dimensions in parallel (batch WebSearch; one WebFetch per source worth quoting; stop at first 1–2 trustworthy sources per dimension). 3. Write `Reading.md`. 4. Distil `Snapshot.md` from it. 5. Confirm to user (type, source count, file paths, biggest gap, the explore suggestions).

## Output A — `Snapshot.md` (Marp deck — ~11 slides light, more in medium/deep)

Marp markdown: frontmatter `marp: true` + shared theme; slides split by `---`. Hard cap ~75 words/slide. Tables/bullets, no prose. In medium/deep, add relevant images and tables (see Images rule).

1. **Cover** — topic · date · type · one-line "what it is" · source count · trust legend
2. **What it is** — definition (quoted) + glossary + don't-confuse-with + before/alternatives
3. **Where it lives** — domain/where it appears + how it works (broad)
4. **Rules** — regulation: rule | year | what it requires | link
5. **Market & players** — player | what they do | positioning | source-flag *(Product → patents + startups + white-space)*
6. **How we got here** — 3–4 milestones + disruption + notable failure
7. **Who to follow** — name | role | one-line POV 💬 | link
8. **What to touch** — demo/instrument/PDF/playground + URL
9. **What research says** — 2–3 papers + consensus
10. **Next: explore** — 3–5 adjacent concepts + why (run penguru on them)
11. **Sources & trust audit** — all sources typed; count per type; gaps marked "not found"

## Output B — `Reading.md`

Same section order, fuller-but-still-light treatment: the quotes, a few more rows, context the deck couldn't fit, complete source list. Compressed essence, not an essay — the document the user returns to.

## Marp theme (shared asset)

One theme so every snapshot looks like one product: clean, high-contrast, readable, Tufte-spirited (whitespace, restrained color, legible tables). Store at `skills/snapshot/assets/penguru.css`, reference from deck frontmatter. No per-deck inline styling.

## Trust mechanism (the value prop)

Every section cites its sources **inline, with working links**. No claim without a link or an explicit "not found".

- **Source-type tags** on every claim: ✅ regulator/official · ◽ independent practitioner · ⚠️ vendor · 💬 KOL opinion.
- **"Not found"** is a valid, required output when no trustworthy source exists. Never fabricate.
- **Epistemic rule** on every number/threshold/"standard": classify ✅ regulatory mandate · ⚠️ empirical finding cited out of context · ❌ convention by inertia. Trace the primary source or flag `[primary source not verified]`. (Pattern preserved in `archived/n4-literature/SKILL.md`.)
- **One primary source per dimension** highlighted.
- Final **trust-audit slide**: source count by type + explicit gap list.

### Anti-hallucination practices (apply these, by name)

Follow established grounded-generation methods — don't improvise:

1. **Retrieval grounding (RAG).** Write only from content actually fetched this run (WebSearch/WebFetch), never from parametric memory. If it wasn't fetched, it isn't in the report. ([survey](https://arxiv.org/abs/2401.01313))
2. **Inline attribution.** Every factual sentence is attributable to a specific fetched source — quote or tightly paraphrase, with the link right there. Prefer quoting for definitions/numbers.
3. **Chain-of-Verification (CoVe).** Before finalizing: draft → generate a short verification question per non-trivial claim → re-check each against the source independently → drop or correct anything that fails. ([Dhuliawala et al., ACL 2024](https://aclanthology.org/2024.findings-acl.212/))
4. **Cross-source corroboration.** Key claims need ≥2 independent sources; if only one (or only vendor) sources it, tag it accordingly.
5. **Abstention.** When sources conflict or are missing, say so ("not found" / "contested") — never resolve it by guessing.

### Link validation (required)

Before a URL goes in either output file, **test that it resolves** (HTTP 2xx/3xx, e.g. `curl -sIL --max-time 15`). A link that fails: drop the source and find another, or mark the claim "source unverified — link dead". No untested links ship. List dead-link drops in the trust-audit.

### Images (medium & deep)

**Images are claims too — same rules.** Only embed an image whose URL was found on a fetched source and passes link validation; attribute it (source + link). Never invent, guess, or hotlink an image URL you didn't see on a real page. Useful image types: architecture/flow diagrams, market maps, charts, product photos, patent figures. **light** mode: an image only if one is genuinely necessary to grasp the topic. **medium/deep**: include the relevant ones. In Marp, embed with `![alt](url)` and a caption crediting the source.

## Brevity (scales with mode)

Read-time budgets (~230 wpm): **light** ≈ 700 words, **medium** ≈ 1,800, **deep** ≈ 2,800. The ~75-words/slide cap stays per slide — medium/deep get *more slides*, not denser ones. Deck slide count ≈ 11 (light) / 16–20 (medium) / 24–30 (deep). Overflow always goes to `Reading.md`, never crammed into a slide.

## Deliverables & acceptance

- `skills/snapshot/SKILL.md` + `skills/snapshot/assets/penguru.css`.
- Register `snapshot` in `.claude-plugin/plugin.json` (`"skills": ["snapshot"]`).
- One real example: `examples/aml-scoring/Snapshot.md` + `Reading.md`. Add a product example (`examples/wifi-pill-dispenser/`) if budget allows.

**Acceptance test:**
1. `/penguru:snapshot AML scoring` (default light) → deck renders, ~11 slides, ~3-min read, every claim has a tested link or "not found", tags present, trust-audit correct, **no external-platform referral**, Next-explore present. Spot-check 3 URLs resolve.
2. `/penguru:snapshot wifi pill dispenser` → Product/Idea auto-detected (no flag); patent + startup scans present; white-space flagged.
3. `/penguru:snapshot ePRO` → cross-domain triage (clinical authorities, not fintech).
4. `/penguru:snapshot AML scoring --medium` → ~8-min read, more sources/dimension, relevant images + tables embedded with working (tested) image URLs and source captions.
