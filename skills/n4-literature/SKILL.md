---
name: n4-literature
description: Finds systematic reviews, meta-analyses, and seminal papers on a topic. Maps the academic evidence layer — what peer-reviewed research says, key authors, and open questions.
trigger: User invokes /penguru:n4-literature <topic>
---

# N4 — Academic Literature

**Topic:** $ARGUMENTS
**Layer:** L4 Empirical Evidence · Depth: ★★★★ · Confidence: medium-high · Decay: slow (years, unless field is active)

---

## Step 0 — Query decomposition and domain triage

### 0a — Extract search terms

"$ARGUMENTS" may be a natural language question, a phrase, or a single keyword. Before searching:

1. **Identify the core concept** — what is the phenomenon being researched?
2. **Extract 3–5 search terms** — short, keyword-style, suitable for academic databases
3. **Generate synonyms and variants** — alternative names, abbreviations, related concepts
4. **Identify what is being asked** — are we looking for causes, effects, methods, prevalence, comparisons?

Example:
- Input: *"what are the main risk factors for SME credit default"*
- Core concept: `SME credit default`
- Search terms: `SME credit default`, `small business credit risk`, `probability of default SME`, `SME loan default`, `credit scoring SME`
- What is asked: causes/predictors → prioritize empirical studies and systematic reviews on predictors

Use these extracted terms (not the original input verbatim) in all queries in Step 1.

### 0b — Domain triage

Classify the core concept by research domain:

| Domain | Key venues |
|--------|-----------|
| Biomedical / health | PubMed, Cochrane, PMC |
| Finance / economics / risk | SSRN, Journal of Finance, Review of Financial Studies |
| Computer science / AI | arXiv (cs.*), ACM DL, IEEE |
| Cross-domain | Semantic Scholar, OpenAlex |

Identify likely publication venues for this specific topic.

---

## Step 1 — Parallel search across all sources

Run all searches simultaneously. Do not wait for one source before querying the next.

### 1a — Scopus (if MCP server configured)
```
search_scopus query="TITLE-ABS-KEY($ARGUMENTS) AND DOCTYPE(re)"          # systematic reviews
search_scopus query="TITLE-ABS-KEY($ARGUMENTS)" SORT(citedby-count,desc) # most cited overall
search_scopus query="TITLE-ABS-KEY($ARGUMENTS) AND PUBYEAR > 2021"       # recent papers
```

### 1b — WebSearch (always)
- `"$ARGUMENTS" systematic review`
- `"$ARGUMENTS" meta-analysis`
- `"$ARGUMENTS" systematic review site:pubmed.ncbi.nlm.nih.gov`
- `"$ARGUMENTS" systematic review site:ssrn.com`
- `"$ARGUMENTS" highly cited [domain]`
- `"$ARGUMENTS" research [current year]`

### 1c — paper-search-mcp (if installed)
```
search_papers query="$ARGUMENTS" filter=systematic_review
search_papers query="$ARGUMENTS" source=semantic_scholar sort=citations
search_papers query="$ARGUMENTS" source=pubmed
```

For each result from any source, extract: title, authors, year, DOI, citation count (if available), abstract or key finding.

---

## Step 2 — Deduplicate

Merge results from all sources by DOI or title+year. Flag duplicates — keep the entry with the most complete metadata. Record how many unique papers each source contributed.

---

## Step 3 — Classify papers

Sort the deduplicated list into three categories:

1. **Systematic reviews / meta-analyses** — ≥2 studies synthesized, explicit methodology
2. **Seminal papers** — high citation count or explicitly foundational in the field
3. **Recent evidence** — published in the last 3 years

If no systematic reviews exist, note it explicitly — absence of systematic evidence is itself a finding.

---

## Step 4 — Full-text analysis

For each paper in the list, attempt full-text access in priority order:

1. **arXiv / bioRxiv / medRxiv** — WebFetch the PDF URL directly
2. **PubMed Central (PMC)** — check `pubmed.ncbi.nlm.nih.gov/[PMID]`
3. **SSRN** — many working papers freely downloadable
4. **Unpaywall** — `unpaywall.org/[DOI]` for legal open-access copy
5. **paper-search-mcp** (if installed) — `download_with_fallback doi="[DOI]"`

If full text accessible, analyze:
- **Methodology:** study design, sample size, data sources, statistical approach
- **Key quantitative findings:** extract numbers from tables/figures
- **Author-stated limitations**
- **Replication status:** retracted, challenged, replicated?

If paywall with no open-access version: note `[abstract only]`.

**Epistemic hygiene rule — apply to every number, threshold, or benchmark encountered:**

When a paper states that X is "the standard", "typically", "commonly accepted", or "widely used":
- Do NOT report it as a fact
- Report it as a **claim by that paper**: *"[Paper Y] states that X is the standard"*
- Check whether the paper cites a primary source for that number
  - If yes: trace it — search `"[number]" [field] origin [primary author]` to verify context
  - If no: flag as `[primary source not verified]`
- Classify the claim:
  - ✅ **Regulatory mandate** — explicitly required by FDA / ICH / EMA / equivalent
  - ⚠️ **Empirical finding cited out of context** — originated in a specific population/domain, applied broadly without validation
  - ❌ **Convention by inertia** — no traceable primary source; repeated across literature without justification

This applies especially to: percentage thresholds, cutoff values, sample size rules, compliance benchmarks, "gold standard" labels.

**Output per paper:**
```
**[Title]** — ✅ full text / ⚠️ abstract only
- Methodology: [design, N, data]
- Key finding: [quantitative result]
- Limitations: [author-stated]
- Replication: [status if known]
- Claims requiring provenance check: [list any threshold/benchmark + classification]
```

---

## Step 5 — Enrich with Scopus metadata

For the top papers identified (systematic reviews + seminal), use Scopus to add citation data and author profiles:

```
get_abstract_details eid="[EID]"       # full metadata, citation count, affiliations
get_author_profile author_id="[ID]"    # h-index, total citations, institution
```

Use this to populate the Key Authors section and verify citation counts.

---

## Step 6 — Open questions and research gaps

Search across sources for what is still unresolved:

- `"$ARGUMENTS" research gap`
- `"$ARGUMENTS" future research directions`
- `"$ARGUMENTS" limitations evidence`
- Also extract gaps stated directly in the papers analyzed

---

## Step 7 — Save output

Create the folder if it does not exist:
```bash
mkdir -p "[vault-path]/Research/$ARGUMENTS"
```

Save to `[vault-path]/Research/$ARGUMENTS/N4-Literature.md`.

**Output format:**

```markdown
# N4-Literature
*[date] · domain: [domain] · [N] unique papers · sources: [list used]*

## Evidence Summary
[2–3 sentences: overall state of evidence — strong/weak/contested/emerging, and why]

## Sources Used
| Source | Papers found | Notes |
|--------|-------------|-------|
| Scopus | N | |
| WebSearch | N | |
| paper-search-mcp | N | |
| Deduplicated total | N | |

## Systematic Reviews & Meta-Analyses

### [Title] — [Authors] ([Year]) ✅/⚠️
**Journal/Venue:** [name] · **Citations:** [N]
**Scope:** [N studies / N participants]
> "[key finding]"
- Methodology: [design]
- Limitations: [author-stated]
Source: [URL]

⚠️ *No systematic reviews found* — if applicable

## Seminal Papers

| Title | Authors | Year | Citations | Full text | Why foundational |
|-------|---------|------|-----------|-----------|-----------------|

## Recent Evidence (last 3 years)

| Title | Authors | Year | Key finding | Full text |
|-------|---------|------|-------------|-----------|

## Key Authors & Research Groups

| Author | Institution | h-index | Focus |
|--------|-------------|---------|-------|

## Open Questions & Research Gaps

- [gap 1]
- [gap 2]
- [gap 3]

## Evidence Map

| Dimension | State | Notes |
|-----------|-------|-------|
| Volume of research | High / Medium / Low | |
| Consensus level | Strong / Contested / Emerging | |
| Recency | Active / Stable / Dormant | |
| Dominant methodology | RCT / Observational / Modelling / Mixed | |

---
*N4 complete — Layer L4 · Confidence: [high/medium/low] · Decay: [rate]*
*Papers: [N total] · Systematic reviews: [N] · Seminal: [N] · Recent: [N] · Full text: [N] · Abstract only: [N]*
```

---

## Step 8 — Confirm

Reply with:
- Sources used and paper count per source
- Systematic reviews found (or explicit note if none)
- Top 2–3 seminal papers with citation counts
- Biggest open question
- File path saved
