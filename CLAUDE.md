# penguru — build brief

penguru is **one one-shot Claude Code skill**: a trusted snapshot research agent. One word in → a source-cited, hallucination-free 3-minute orientation on any topic, broad across every relevant dimension (definitions, market, regulation, history, people, patents…), shallow per dimension.

It is **not** a deep research tool and **not** a node system. One skill, one report, self-contained. It does **not** refer the user to other platforms (Undermind/Elicit/Perplexity) — the report stands on its own and only suggests *adjacent topics to snapshot next*.

The old per-dimension skills (`n1-definitions`, `n4-literature`) are **archived** under `archived/` — kept for reference (trust patterns), not shipped.

## Current task

Build the one skill **`/penguru:snapshot <topic>`**. Full design in [`skills/snapshot/SPEC.md`](skills/snapshot/SPEC.md) — read it first, it is the contract.

In short: triage the topic's type, run a **light** web pass over the relevant dimensions (no MCP, no paid APIs), produce two files in `./penguru-research/<topic>/`:
- `Snapshot.md` — a Marp deck, ~11 slides, a 3-minute skim
- `Reading.md` — a reference document, fuller light detail per dimension

Input may be a concept (`AML scoring`, `ePRO`) **or a concrete product idea** (`golf`, `ergonomic chair`, `wifi pill dispenser`) — products trigger a patent scan + startup scan, **auto-detected by the skill (zero friction, the user never activates it)**. See SPEC.

## Principles (non-negotiable)

1. **Never trust parametric knowledge.** Write only from sources fetched this run (RAG grounding); every claim carries an inline **working** link; no source → "not found", never invent. Apply **Chain-of-Verification** before finalizing, and **test every link resolves** before it ships. Named practices in SPEC → *Anti-hallucination practices* + *Link validation*.
2. **Respect working memory.** 3-minute read. Hard brevity cap per slide (~75 words). Tables/bullets, not prose.
3. **Two artefacts.** The deck is a *lesson* — beautiful, Tufte-clean, skimmed once. `Reading.md` is a *reference document* — compressed essence, returned to later.
4. **One primary source per dimension** — the single most trustworthy source, not just a list.
5. **Glossary is first-class** — define every term of art.
6. **Self-contained, no external referrals.** The only forward-pointer is the *Next: explore* section (adjacent concepts to snapshot next, internal).
7. **Optional angle.** A user may pass a lens (PM/engineer/investor); tilt emphasis.
8. **Reuse.** One shared Marp theme so every snapshot looks like one product. Reuse the trust patterns in `archived/n1-definitions` (source-typing) and `archived/n4-literature` (epistemic rule).

## Conventions

- Skill file format: follow the structure of the archived skills (`archived/n1-definitions/SKILL.md`): frontmatter `name`/`description`/`trigger`, numbered Steps, an embedded output-format template, a confirm step.
- Output path is portable: `./penguru-research/<topic>/` in the current working directory.
- Trust tags and the "not found" rule apply to every dimension.
- When done: set `.claude-plugin/plugin.json` `"skills": ["snapshot"]`, and ship at least one real example under `examples/aml-scoring/`.

## Out of scope

No paid APIs, no MCP. Patent/startup/literature scans are web-light only (Google Patents, YC, Product Hunt, public web). No new skills beyond `snapshot` — the project closes on this one skill.
