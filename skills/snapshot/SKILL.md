---
name: snapshot
description: One-shot trusted orientation on any topic — broad-shallow, source-cited, hallucination-free. Three depth modes: --light (default, ~3 min · ~11 slides), --medium (~8 min · 16–20 slides), --deep (~12 min · 24–30 slides). Auto-detects product/idea topics (no user flag needed).
trigger: User invokes /penguru:snapshot <topic> [--light|--medium|--deep]
---

# penguru — snapshot

**Topic:** $ARGUMENTS  
**Mode:** detect flag in $ARGUMENTS — `--deep` → deep · `--medium` → medium · `--light` → light.

**If NO mode flag is present in $ARGUMENTS, do not silently default.** Ask the user first, before any search:

> Use `AskUserQuestion` — header "Depth", one question, three options:
> - **Light (~3 min)** — core dimensions, 1–2 sources each, ~11 slides *(recommended)*
> - **Medium (~8 min)** — all dimensions fuller, 2–3 sources, images + tables, 16–20 slides
> - **Deep (~12 min)** — deepest one-shot, 3+ cross-checked sources, rich images, 24–30 slides

A flag in $ARGUMENTS overrides the prompt (skip the question). Trust rules are identical in every mode — only volume/images scale.

---

## Step 0 — Triage (classify topic, set dimension weights)

Before any search, reason about **"$ARGUMENTS"** (strip mode flag if present):

### 0a — Topic type

Classify as exactly one of:

| Type | Examples |
|------|---------|
| Technology/Tool | LLM, ePRO, AML scoring |
| Methodology/Practice | Agile, underwriting, DCF |
| Market/Sector | BNPL, eCOA market |
| Regulatory framework | GDPR, FDA PRO Guidance |
| Scientific concept | Probability of Default, entropy |
| **Product/Idea** | golf club, ergonomic chair, wifi pill dispenser |

### 0b — Product/Idea auto-detection (zero friction)

If the topic is a **concrete tangible product or idea** (something you could hold, buy, or patent):
- Set `PRODUCT_MODE = true`
- "Market & players" → retitled **"Who makes it + IP"**
- Add **Patent scan** + **Startup scan** dimensions
- Flag the gap / white space in the report
- User never sets this — the skill decides silently

Otherwise `PRODUCT_MODE = false`.

### 0c — Domain authorities & synonyms

- Identify the field's regulatory/standards bodies (the "FDA" for this domain)
- List 3–5 synonyms or variant spellings to include in queries
- Note whether the topic is **stable** (years-old consensus) or **emerging** (rapidly evolving)

### 0d — Dimension checklist for this mode

Mark each dimension **IN** or **SKIP** based on topic type and mode:

| Dimension | light | medium | deep | Product mode |
|-----------|-------|--------|------|--------------|
| Definitions & glossary | ✅ | ✅ | ✅ | ✅ |
| Domain & how it works | ✅ | ✅ | ✅ | ✅ |
| Alternatives / before | ✅ | ✅ | ✅ | ✅ (competing products) |
| Market & players | ✅ | ✅ | ✅ | → "Who makes it + IP" |
| Regulation & standards | ✅ | ✅ | ✅ | light if not heavy reg domain |
| Adjacent concepts | ✅ | ✅ | ✅ | ✅ |
| History | skip | ✅ | ✅ | skip |
| People to follow | skip | ✅ | ✅ | skip |
| Artefacts | ✅ | ✅ | ✅ | ✅ |
| Research snapshot | ✅ | ✅ | ✅ | ✅ |
| Patent scan | skip | skip | skip | ✅ always |
| Startup scan | skip | skip | skip | ✅ always |

---

## Step 1 — Parallel web search (batch per dimension)

**Sources/dimension:** light = 1–2 · medium = 2–3 · deep = 3+

Run all WebSearch queries simultaneously (batch). Use synonyms from Step 0c to widen queries. Do **not** wait for one before starting the next.

### Queries to run (adapt to topic; strip mode flag from $ARGUMENTS first):

```
# Definitions & glossary
"$TOPIC" definition site:[authority-1].org OR site:[authority-2].org
"$TOPIC" glossary [domain]
"$TOPIC" meaning [domain] official

# Domain & how it works
"$TOPIC" how it works [domain]
"$TOPIC" methodology explained [domain]

# Alternatives / before
"$TOPIC" alternatives compared
"$TOPIC" vs [main alternative]

# Market & players
"$TOPIC" vendors market leaders [year]
"$TOPIC" companies landscape [year]

# Regulation & standards
"$TOPIC" regulation [regulator] [year]
"$TOPIC" compliance standard [domain body]

# Adjacent concepts
"$TOPIC" related concepts [domain]
"$TOPIC" confused with

# History (medium/deep only)
"$TOPIC" history timeline origin

# People to follow (medium/deep only)
"$TOPIC" expert thought leader [domain]

# Artefacts
"$TOPIC" demo tool playground open source
"$TOPIC" paper PDF open access

# Research snapshot
"$TOPIC" systematic review
"$TOPIC" seminal paper [domain]

# Patent scan (PRODUCT_MODE only)
"$TOPIC" patents site:patents.google.com
"$TOPIC" patent landscape assignee

# Startup scan (PRODUCT_MODE only)
"$TOPIC" startup site:ycombinator.com OR site:producthunt.com
"$TOPIC" startup funding crunchbase
```

---

## Step 2 — WebFetch trustworthy sources

For each dimension, pick the **most promising result** from Step 1 and WebFetch it to extract exact quotes, numbers, and metadata.

**Source-type tags to apply on every claim:**
- ✅ Regulator / official body
- ◽ Independent practitioner (non-vendor)
- ⚠️ Vendor source (include but tag)
- 💬 KOL opinion

**Primary source per dimension:** mark the single most trustworthy source fetched. Prefer official/independent over vendor.

**Cross-source corroboration:** any key claim (number, threshold, "standard", market-leader status) needs ≥2 independent sources. If only one source — or only vendor sources — back it, tag the claim accordingly and note "single-source" / "vendor-only". Don't present uncorroborated claims as settled.

**Epistemic rule** — when a source states something is "standard", "typical", "widely used": classify the claim as:
- ✅ Regulatory mandate — required by a named authority
- ⚠️ Empirical finding (specific context, don't overgeneralise)
- ❌ Convention by inertia — no traceable primary source

If no trustworthy source found for a dimension → output `not found` for that dimension. Never fabricate.

---

## Step 3 — Chain-of-Verification (mandatory, before writing)

Before writing any output file:

1. Draft the key claims list (one line each)
2. For each non-trivial claim, generate a verification question: *"Does [source] actually say [claim]?"*
3. Re-check each against the fetched content (do not rely on memory)
4. Drop or correct anything that fails — replace with `not found` or `[primary source not verified]`
5. Flag conflicting sources explicitly: "contested — sources disagree"

---

## Step 4 — Link validation (mandatory)

Before writing any URL to either output file:

```bash
curl -sIL --max-time 15 "<url>" | head -5
```

**Rule:** keep only HTTP 2xx or 3xx. If a link fails:
- Drop the source, find another, or mark claim `source unverified — link dead`
- List every dead-link drop in the trust-audit slide

**Images — MANDATORY, not optional.** medium/deep MUST embed **≥2 relevant images**; light ≥1 if the topic is even slightly visual. A snapshot with zero images is a failure for any topic a human would expect to *see* (a product, an org chart, a process, an anatomy/biology/physical thing — "if asked about the cell, obviously show a photo of the cell"). Abstract topics still have diagrams (process flow, risk matrix, architecture, timeline). Rules:
- Only embed an image URL found on a fetched source that passes curl validation returning `image/*` content-type. Never guess or invent image URLs.
- **Match the image to the topic type:**
  - *Product / competitor topic* → pull a **real product screenshot from the competitor's own page**: WebFetch the page, grab the `og:image` meta and/or product-UI `<img>` CDN src (e.g. sanity.io, the vendor's `/media/…` path — prefix relative paths with the host), then curl-validate. Embed the 1–2 most on-topic players' UIs, captioned `Source: [vendor] ⚠️ (vendor screenshot)`.
  - *Research-heavy topic* → pull a **figure from a paper**: try the arXiv HTML render (`arxiv.org/html/<id>` or `ar5iv.labs.arxiv.org/html/<id>`) and take a real `<img>` figure src. If the paper has no HTML version (404) or the figure URL can't be validated, **don't force it** — say so in the confirm step.
  - *Otherwise* → diagram from Wikimedia/Commons (process flow, risk matrix, timeline, architecture).
- **Hotlink-stable sources first:** `upload.wikimedia.org` (Wikipedia/Commons) is reliable and CC-licensed — prefer it for generic diagrams. Vendor CDNs (sanity.io, etc.) usually hotlink fine once validated; some block — always curl-check.
- Validate with: `curl -sIL --max-time 15 -o /dev/null -w "%{http_code} %{content_type}" "<url>"` → keep only `200 image/*`.
- Every image gets a caption that says what it shows + `Source: … (license)`.
- If you genuinely cannot find ≥2 validated images, say so explicitly in the confirm step (don't silently ship zero).

---

## Step 5 — Write Reading.md

**Output dir (`$OUT`):** `/Users/adrianofontanari/Library/Mobile Documents/iCloud~md~obsidian/Documents/AdryOS/Resources/Penguru` — notes live inside the Obsidian vault so wikilinks resolve and they show up in Obsidian. Create `$OUT` if it doesn't exist.

Save to: `$OUT/$TOPIC - Reading.md`  
(no per-topic subfolder — both notes live flat in `$OUT`, named after the keyword.)

**Cross-link:** the first line of the note must be an Obsidian wikilink to its companion deck: `↪ [[$TOPIC - Snapshot]]`

Same section order as the deck, fuller treatment. Quotes, a few more rows, context the deck couldn't fit, complete source list. Not an essay — compressed essence the user returns to. **Word budget: light ≈ 700 · medium ≈ 1,800 · deep ≈ 2,800.**

**Output template:**

```markdown
↪ [[$TOPIC - Snapshot]]

# $TOPIC — Reading
*[date] · mode: [light|medium|deep] · type: [topic type] · [N] sources · [N] dimensions covered*

## Definitions & Glossary

**Primary source:** [name · URL] ✅/◽/⚠️

> "[exact quote — definition]"
Source: [title] · [URL] ✅/◽/⚠️

### Glossary
| Term | Definition | Source |
|------|------------|--------|

## Domain & How It Works

[2–4 sentences, fetched content only, no parametric memory]
Source: [URL] ✅/◽/⚠️

## Alternatives / Before

| Alternative | What it is | Key difference | Source |
|-------------|------------|----------------|--------|

## Market & Players  *(Product mode: "Who Makes It + IP")*

**Map the market, don't just list it.** Two-step, mandatory in medium/deep:

1. **Categories first** — segment the space into 3–6 functional categories (what each *role* does in the stack), then place players into them. Don't dump an undifferentiated vendor list.
2. **Feature map** — for each player give concrete **key features**, a one-line **differentiator**, and **positioning**. Distinguish adjacent categories (e.g. signal-generator vs decision-engine vs data-platform) so the user sees *who competes on what*.

**Query width:** beyond incumbent names + "best X software" lists (incumbent-biased — they miss recent late-stage players), always also run: `"$TOPIC" decisioning OR orchestration platform`, `"$TOPIC" startup funding series crunchbase`, and the direct names of any adjacent-category vendors you know. 4–8 players. Funding/figures from public web only — tag vendor sources ⚠️.

```
### Categories
| Category | Role in the stack |
|----------|-------------------|

### Feature map
| Player | Category | Key features | Differentiator | Positioning | Source |
|--------|----------|--------------|----------------|-------------|--------|
```

End with a **positioning split**: one line per category naming the axis it competes on, + note that a real stack usually combines ≥2 categories.

*(Product mode adds: IP/white space row)*

## Regulation & Standards

| Body | Document | Year | What it requires | Link |
|------|----------|------|-----------------|------|

## Adjacent Concepts

| Concept | Distinction from $TOPIC | Source |
|---------|------------------------|--------|

## History *(medium/deep only)*

3–4 milestones + one disruption event + any notable failure (mark which row is which).

| Year | Event | Kind (milestone / disruption / failure) | Source |
|------|-------|------------------------------------------|--------|

## People to Follow *(medium/deep only)*

| Name | Role | POV 💬 | Link |
|------|------|--------|------|

## Artefacts

| Type | Name/Link | Notes |
|------|-----------|-------|

## Research Snapshot

| Paper | Authors | Year | Key finding | Epistemic class | Link |
|-------|---------|------|-------------|-----------------|------|

**Consensus:** [strong / contested / emerging] — [1 sentence why]

## Patent Scan *(Product mode only)*

| Patent | Assignee | Year | Claim summary | Link |
|--------|----------|------|--------------|------|

**White space:** [gap identified or "not found"]

## Startup Scan *(Product mode only)*

| Company | Stage | What they build | Source |
|---------|-------|----------------|--------|

## Next: Explore

- **[Concept 1]** — [why relevant to $TOPIC]
- **[Concept 2]** — [why relevant]
- **[Concept 3]** — [why relevant]
*(Run `/penguru:snapshot <concept>` to go deeper)*

---
## Sources

| # | Title | URL | Type | Status |
|---|-------|-----|------|--------|
| 1 | | | ✅/◽/⚠️/💬 | live |

*Dimensions with "not found": [list or "none"]*
*Dead links dropped: [list or "none"]*
```

---

## Step 6 — Distil Snapshot.md (Marp deck)

Save to: `$OUT/$TOPIC - Snapshot.md` (flat in `$OUT` = `…/AdryOS/Resources/Penguru`, no subfolder, named after the keyword)

**Cross-link:** add an Obsidian wikilink back to the reading note right after the title slide's metadata line: `↪ [[$TOPIC - Reading]]`. Write it as **plain visible text**, never inside an HTML comment (`<!-- ... -->`) — Obsidian does not render wikilinks inside comments.

**Theme wiring:** the deck frontmatter sets `theme: penguru`. The `penguru` theme lives in `skills/snapshot/assets/penguru.css` (registered via its `/* @theme penguru */` header). To render, point Marp at that file with `--theme-set`. Tell the user the exact command after saving, e.g.:

```bash
marp "$OUT/$TOPIC - Snapshot.md" --theme-set /Users/adrianofontanari/Projects/Penguru/skills/snapshot/assets/penguru.css --html -o "$OUT/$TOPIC - Snapshot.html"
# or live preview:
marp -p -w "$OUT/$TOPIC - Snapshot.md" --theme-set /Users/adrianofontanari/Projects/Penguru/skills/snapshot/assets/penguru.css
```

(Without `--theme-set` Marp can't find `penguru` and falls back to the default theme.)

**Marp frontmatter:**
```yaml
---
marp: true
theme: penguru
paginate: true
---
```

**Slide count:** light ≈ 11 · medium 16–20 · deep 24–30  
**Hard cap:** ~75 words per slide. Tables/bullets — no prose paragraphs.  
**Images:** MANDATORY (see Step 4) — medium/deep embed ≥2 validated images, light ≥1 if visual. Prefer Wikimedia/CC diagrams; validated URLs only, caption + source. Marp inline sizing: `![w:340](url)`. Zero images = failure.

**Slide structure (light):**

```markdown
---
marp: true
theme: penguru
paginate: true
---

# $TOPIC
*[date] · [topic type] · [N] sources · [mode]*
↪ [[$TOPIC - Reading]]

> "[one-line what it is — fetched definition, quoted]"

Trust legend: ✅ official · ◽ independent · ⚠️ vendor · 💬 opinion

---

## What it is

**[Term]:** [definition — quoted from primary source]
Source: [URL] ✅

**Don't confuse with:** [adjacent term] — [1-line distinction]

### Glossary
| Term | Definition |
|------|------------|

---

## Where it lives

[2–3 bullets: domain · who uses it · where it appears]
Source: [URL] ✅/◽

**How it works (broad strokes):**
[2–3 bullets from fetched content]

---

## Rules

| Body | Document | Year | What it requires |
|------|----------|------|-----------------|
| [✅ body] | [doc] | [year] | [requirement] · [link] |

*not found: [any gaps]*

---

## Market & players  *(Product: "Who makes it + IP")*

| Player | What they do | Tag |
|--------|-------------|-----|

*(Product: + white space row)*

---

## How we got here  *(medium/deep — skip light)*

3–4 milestones + 1 disruption + notable failure if any.

| Year | Event | Kind |
|------|-------|------|

---

## Who to follow  *(medium/deep — skip light)*

| Name | Role | POV 💬 |
|------|------|--------|

---

## What to touch

| Type | Link | Notes |
|------|------|-------|

---

## What research says

| Paper | Year | Key finding | Class |
|-------|------|-------------|-------|

**Consensus:** [strong / contested / emerging]

---

## Next: explore

- **[Concept 1]** — [why]
- **[Concept 2]** — [why]
- **[Concept 3]** — [why]

*Run `/penguru:snapshot <concept>` on any of these*

---

## Sources & trust audit

| Type | Count | Examples |
|------|-------|---------|
| ✅ Official/regulator | N | |
| ◽ Independent practitioner | N | |
| ⚠️ Vendor | N | |
| 💬 KOL opinion | N | |
| **Total** | **N** | |

**Gaps ("not found"):** [list]  
**Dead links dropped:** [list or "none"]
```

---

## Step 7 — Confirm

Reply with:
- Topic type identified + PRODUCT_MODE status
- Mode applied (light / medium / deep)
- Source count by type (✅ / ◽ / ⚠️ / 💬)
- Dimensions covered vs skipped
- Biggest gap ("not found" items)
- Dead links dropped (if any)
- Files saved: `$OUT/$TOPIC - Snapshot.md` · `$OUT/$TOPIC - Reading.md` (in the Obsidian vault, cross-linked via wikilinks)
- Render command (the `marp … --theme-set …` line from Step 6) so the user can build the deck
- Next: explore suggestions (3–5 concepts)
