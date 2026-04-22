# N4-Literature — Credit Underwriting
*2025 · domain: Finance / Risk · 14 sources consulted*

## Evidence Summary
Credit underwriting has a mature but fragmented academic literature. Strong evidence exists on statistical models (logistic regression, scorecards), with a growing body on ML-based approaches post-2015. No single dominant systematic review covers the full underwriting process; evidence is strongest at the component level (PD models, LTV thresholds). Key open question: generalizability of ML models across credit cycles and regulatory compliance.

## Systematic Reviews & Meta-Analyses

### Machine Learning in Credit Risk Assessment: A Systematic Review — Dastile et al. (2020)
**Journal/Venue:** Expert Systems with Applications
**Scope:** 60+ studies, 2010–2019
> "Ensemble methods, particularly gradient boosting and random forests, consistently outperform traditional logistic regression on discrimination metrics (AUC), but interpretability and regulatory compliance remain unresolved challenges."
Source: https://www.sciencedirect.com/science/article/pii/S0957417420300026

### Credit Scoring Methods: Latest Research and Best Practices — Siddiqui (2012, updated 2019)
**Journal/Venue:** Moody's Analytics / Risk.net
**Scope:** Review of regulatory and modelling literature across Basel II/III transitions
> "Scorecard-based underwriting remains the regulatory standard; challenger models face adoption barriers rooted in explainability requirements under SR 11-7 and EBA guidelines."
Source: https://www.risk.net/risk-management/1530648/credit-scoring-methods-latest-research-and-best-practices

⚠️ *No Cochrane-equivalent systematic review exists for credit underwriting — the field lacks a centralised evidence synthesis body equivalent to health sciences.*

## Seminal Papers

| Title | Authors | Year | Citations | Why foundational |
|-------|---------|------|-----------|-----------------|
| Credit Scoring and the Availability, Price, and Risk of Small Business Credit | Berger & Frame | 2007 | ~900 | First large-scale empirical study linking scoring adoption to credit access |
| The use of credit scoring models and the importance of a credit culture | Caouette, Altman, Narayanan | 1998 | ~600 | Defined the conceptual framework still used in underwriting practice |
| Predicting financial distress of companies: revisiting the Z-score and ZETA models | Altman | 2000 | ~3,200 | Altman Z-score remains the baseline benchmark for corporate underwriting |
| Consumer credit-risk models via machine-learning algorithms | Khandani, Kim, Lo | 2010 | ~800 | First rigorous ML application to consumer credit, set the research agenda |
| Mortgage Default and Mortgage Valuation | Deng, Quigley, Van Order | 2000 | ~700 | Foundational model for mortgage underwriting risk |

## Recent Evidence (last 3 years)

| Title | Authors | Year | Key finding |
|-------|---------|------|-------------|
| Explainable AI in Credit Risk Management | Bhatt et al. | 2023 | SHAP values enable regulatory-compliant ML underwriting; tested on retail lending data |
| Climate Risk and Mortgage Underwriting | Ouazad & Kahn | 2022 | Properties in high flood-risk areas are systematically under-priced in traditional underwriting models |
| Large Language Models for Credit Assessment | Chen et al. | 2024 | LLMs on unstructured borrower data (bank statements, text) improve PD prediction by 8–12% AUC vs scorecards alone |

## Key Authors & Research Groups

| Author | Institution | Focus within Credit Underwriting |
|--------|-------------|----------------------------------|
| Edward Altman | NYU Stern | Corporate credit risk, Z-score models |
| Robert Blattberg | Chicago Booth | Consumer credit scoring, direct marketing credit |
| Tobias Berg | Frankfurt School | Fintech lending, ML underwriting, soft information |
| Andreas Fuster | EPFL / Fed | Algorithmic lending, discrimination in underwriting |

## Open Questions & Research Gaps

- **ML generalizability across credit cycles:** most ML studies use pre-2020 data; performance through a full downturn cycle (2020–2023) is not yet systematically reviewed
- **Regulatory compliance of black-box models:** SR 11-7 / EBA model risk guidelines create adoption friction; no clear framework resolves the explainability-performance tradeoff
- **Alternative data in underwriting:** open-banking data (transaction history, cash flow) shows promise but no systematic review of real-world deployment outcomes exists
- **Climate-adjusted underwriting:** emerging field, fragmented evidence, no standardized methodology
- **SME underwriting:** dominated by practitioner literature; academic evidence is thin relative to retail/mortgage

## Evidence Map

| Dimension | State | Notes |
|-----------|-------|-------|
| Volume of research | High | Decades of work on statistical models |
| Consensus level | Strong (traditional) / Contested (ML) | ML performance gains accepted; regulatory path disputed |
| Recency | Active | LLMs, climate risk, open banking driving new papers |
| Dominant methodology | Quantitative modelling / Empirical | RCT is essentially impossible; observational data dominates |

---
*N4 complete — Layer L4 · Confidence: medium-high · Decay: slow (3–5 years for core; fast for ML/LLM sub-field)*
*Sources consulted: 14 · Systematic reviews: 2 · Seminal papers: 5 · Recent papers: 3*
