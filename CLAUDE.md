# penguru — build brief

penguru is a Claude Code plugin: **a trusted snapshot research agent.** One word in → a source-cited, hallucination-free 3-minute orientation on any topic, broad across every dimension (definitions, market, regulation, history, KOLs, patents…), shallow per dimension.

It is **not** a deep research tool. Deep research is a crowded space (Undermind, Elicit, Perplexity/OpenAI Deep Research). penguru owns the gap before them: the first 3 minutes on a topic you know nothing about. It hands off to the deep tools at the end.

## Current task

Build the skill **`/penguru:snapshot <topic>`**. Full design is in [`skills/snapshot/SPEC.md`](skills/snapshot/SPEC.md) — read it first, it is the contract.

In short: the skill triages the topic's ontological type, runs a **light** pass over all 12 research nodes (web only, no MCP required), and produces two files in `./penguru-research/<topic>/`:
- `Snapshot.md` — a Marp slide deck, ~11 slides, a 3-minute skim
- `Reading.md` — a reference document with the fuller (still light) detail per node

The input can be an abstract concept (`AML scoring`, `ePRO`) **or a concrete product idea** (`golf`, `ergonomic chair`, `wifi pill dispenser`). For product ideas the triage activates a patent scan and a startup scan. See SPEC.

## Principles (non-negotiable)

1. **Never trust parametric knowledge.** Every factual claim carries an inline source (URL + date). No source → write "not found", never invent.
2. **Respect working memory.** 3-minute read. Hard brevity cap per slide (~75 words). Tables and bullets, not prose. Difficulty is the enemy for orientation.
3. **Two artefact types.** The deck is a *lesson* — beautiful, Tufte-clean, skimmed once. `Reading.md` is a *reference document* — compressed essence, returned to later.
4. **One primary source per dimension** — surface the single most trustworthy source, not just a list.
5. **Glossary is a first-class reference.** Define every term of art.
6. **Optional angle/mission.** A user may pass a lens (PM / engineer / investor); tilt emphasis accordingly.
7. **Knowledge → wisdom handoff.** End by routing the user onward: KOLs/communities to follow, and the deep tools (Undermind/Elicit/Perplexity) with ready-made queries.
8. **Reuse.** One shared Marp theme so every snapshot looks like one product. Reuse the trust DNA already in `skills/n1-definitions` (source-typing: ✅ regulator / ◽ independent / ⚠️ vendor) and `skills/n4-literature` (epistemic rule: classify every threshold as regulatory mandate / empirical finding / convention by inertia).

## Conventions

- Skill file format: match `skills/n1-definitions/SKILL.md` and `skills/n4-literature/SKILL.md` (frontmatter `name`/`description`/`trigger`, then numbered Steps, then an embedded output-format template, then a confirm step).
- Output path is portable: `./penguru-research/<topic>/` in the current working directory. Also fix the unresolved `[vault-path]` placeholder in the N1/N4 skills to this convention.
- Trust tags and the "not found" rule above apply to every node.
- After building: register `snapshot` (and the currently-missing `n4-literature`) in `.claude-plugin/plugin.json`, and ship at least one real example under `examples/` (e.g. `examples/aml-scoring/`).

## Out of scope for this skill

No paid APIs. Patent/startup/literature scans are web-light only (Google Patents, YC, Product Hunt, public web). The deep nodes (Scopus, Crunchbase) stay in the deep skills, not in snapshot.
