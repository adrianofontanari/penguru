# penguru

![penguru](penguru-icon.png)

> Turn a single word into deep domain expertise.

LLMs are fast but untrustworthy when you need to build real expertise. They hallucinate sources, flatten nuance, and can't tell a regulator from a vendor. **penguru** is a Claude Code plugin built around a node architecture: each node targets a specific dimension of knowledge, from definitions to market intelligence to regulatory standards.

The release plan is incremental. This first release covers definitions - the foundational node that grounds any concept in authoritative sources, separates what regulators say from what practitioners say, and flags commercial bias for what it is. Future releases will add nodes progressively, building toward a systematic and scientific approach to domain knowledge.

## Skills

### `/penguru:n1-definitions <topic>`

Produces a structured definitions document with:
- ≥2 definitions from independent authoritative sources (regulators, standards bodies)
- Industry definitions — Tier 1 (independent) and Tier 2 (vendor, flagged)
- Terminological distinctions (what gets confused with this concept)
- Minimum glossary
- Who standardizes the concept and where

**Example output:** [`credit underwriting`](examples/credit-underwriting-N1.md)

## Install

```bash
# Clone
git clone https://github.com/adrianofontanari/penguru ~/Projects/penguru

# Use in any Claude Code project
claude --plugin-dir ~/Projects/penguru
```

Then invoke with:
```
/penguru:n1-definitions credit underwriting
/penguru:n1-definitions probability of default
/penguru:n1-definitions AML transaction monitoring
```

## Output example

```
## Authoritative Definitions

### EBA — GL/2020/06 Loan Origination and Monitoring
> "Credit is granted to borrowers who, to the institution's best knowledge
> at the time of granting the credit, will be able to fulfil the terms and
> conditions of the credit agreement."

### OCC (Office of the Comptroller of the Currency)
> "Underwriting refers to the terms and conditions under which [banks]
> extend or renew credit, such as financial and collateral requirements,
> repayment programs, maturities, pricing, and covenants."

## Industry Definitions

### Fintegrationfs — Practitioner (independent)
> "Underwriting is the process of assessing whether a borrower is a suitable risk."

### Underwrite.ai — [vendor]
> "Credit underwriting is the process of assessing whether a borrower is
> likely to repay a loan."
⚠️ vendor — not an independent source
```

## Roadmap

- [x] N1 — Foundational Definitions
- [ ] N2 — Domain Context
- [ ] N6 — Technical Layer
- [ ] N7 — Regulatory & Standards
- [ ] Full T-Shaped Research Agent (12 nodes)

## Background

Built on the T-Shaped Research Agent framework — a faceted knowledge model inspired by DIKW (Ackoff), Ranganathan's faceted classification, and Information Foraging Theory (Pirolli & Card).
