---
name: n1-definitions
description: Finds authoritative definitions for any concept from ≥2 independent sources (official + industry), produces terminological distinctions, minimum glossary, and saves structured output.
trigger: User invokes /penguru:n1-definitions <topic>
---

# N1 — Foundational Definitions

**Topic:** $ARGUMENTS
**Layer:** L1 Synchronic · Depth: ★★★ · Confidence: high · Decay: stable (years)

---

## Step 0 — Identify ontological type and authoritative sources

Before searching, reason about the topic "$ARGUMENTS":

1. **Ontological type** — classify as one of:
   - Technology/Tool (e.g. LLM, blockchain, ePRO)
   - Methodology/Practice (e.g. Agile, credit underwriting, DCT)
   - Regulatory framework (e.g. GDPR, FDA PRO Guidance)
   - Scientific concept (e.g. Risk Score, Probability of Default)
   - Market/Sector (e.g. fintech BNPL, eCOA market)

2. **Authoritative sources for the domain** — based on type, identify who standardizes this concept:
   - Life sciences / clinical: FDA, EMA, WHO, ISPOR, ICH, ISO
   - Fintech / risk / banking: BIS, EBA, BCBS, FSB, OCC, FDIC, Federal Reserve
   - Tech / software: ISO/IEC, NIST, W3C, IETF
   - Other: domain-specific trade associations and standards bodies

3. **Industry sources** — two tiers, both valid but weighted differently:
   - **Tier 1 — independent:** practitioner associations, non-vendor media (e.g. acams.org, fraud.net, fintechtakes.com)
   - **Tier 2 — vendor:** glossaries and blogs from market players — include but tag `[vendor]` in output

4. **Synonyms and variants** — list 3-5 alternative terms or abbreviations to include in queries

---

## Step 2 — Search for official definitions

Run WebSearch with queries adapted to the identified domain:

- `"$ARGUMENTS" definition site:[authority-1] OR site:[authority-2]`
- `"$ARGUMENTS" definition [domain-specific body] official`
- `"$ARGUMENTS" glossary [domain]`
- `[main synonym] definition authoritative`

For each relevant result, use WebFetch to extract the exact definition with full citation.

**Goal:** ≥2 definitions from **independent** authoritative sources.

---

## Step 2 — Search for industry definitions

Run WebSearch for industry sources:

- `"$ARGUMENTS" definition site:[tier1-source-domain]`
- `"$ARGUMENTS" practitioner definition independent`
- `"$ARGUMENTS" [domain-vendor-leader] definition` → tag as `[vendor]`

---

## Step 3 — Terminological distinctions

Search for critical distinctions between "$ARGUMENTS" and similar or commonly confused terms:

- `"$ARGUMENTS" vs [similar-term] difference`
- `difference between "$ARGUMENTS" and [alternative]`

---

## Step 4 — Who standardizes

Identify which body issued the most-cited definition, whether a formal standard exists (ISO number, FDA guidance, etc.), and year of first standardization if available.

---

## Step 5 — Save output

Create the folder if it does not exist:
```bash
mkdir -p "[vault-path]/Research/$ARGUMENTS"
```

Save to `[vault-path]/Research/$ARGUMENTS/N1-Definitions.md`.

**Output format:**

```markdown
# N1-Definitions
*[date] · ontological type: [type] · [N] sources consulted*

## Ontological Type
[type] — [1 sentence explaining why]

## Authoritative Definitions

### [Body A] — [Year]
> "[exact quote]"
Source: [title] · [URL]

### [Body B] — [Year]
> "[exact quote]"
Source: [title] · [URL]

## Industry Definitions

### [Tier 1 source] — Practitioner (independent)
> "[quote]"
Source: [title] · [URL]

### [Tier 2 source] — [vendor]
> "[quote]"
Source: [URL] ⚠️ *vendor — not an independent source*

## Who Standardizes
| Body | Document | Year | Authority | Scope |
|------|----------|------|-----------|-------|

## Terminological Distinctions
| Term | Definition | Difference from $ARGUMENTS |
|------|------------|---------------------------|

## Minimum Glossary
| Term | Definition | Source |
|------|------------|--------|

---
*N1 complete — Layer L1 · Confidence: high · Decay: stable*
*Sources consulted: [N] · Official definitions: [N] · Industry: [N independent] + [N vendor]*
```

---

## Step 6 — Confirm

Reply with:
- Ontological type identified
- Authoritative sources found (body name + document)
- File path saved
- Any issues (source inaccessible, definition not found, etc.)
