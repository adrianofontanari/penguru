# AML Scoring — Reading
*2026-06-27 · mode: light · type: Technology/Practice · 13 sources consulted · 8 dimensions covered*

PRODUCT_MODE: false — concept, not tangible product.

---

## Definitions & Glossary

**Primary source:** AML Network Glossary · https://amlnetwork.org/aml-glossary/risk-score/ ◽ independent practitioner

> "A numerical or categorical value that quantifies the money laundering and terrorist financing risk posed by a customer, transaction, product, or geographic area."
Source: AML Network Glossary · https://amlnetwork.org/aml-glossary/risk-score/ ◽

Extended definition (vendor context):
> "A continuous measurement system that aggregates multiple weak signals across a customer's transaction history to assign a numeric risk level, replacing binary red-flag checklists."
Source: Precisa · https://precisa.in/blog/aml-risk-scoring-suspicious-transaction-monitoring/ ⚠️ vendor

Also used as (from Ondato):
> "A structured process of identifying, scoring, and documenting fraud or money laundering risks associated with customers, products, geographies, channels, and transactions."
Source: Ondato · https://ondato.com/blog/aml-risk-assessment/ ⚠️ vendor

### Glossary
| Term | Definition | Source |
|------|------------|--------|
| ML/TF | Money Laundering / Terrorist Financing | FATF ✅ (referenced) |
| KYC | Know Your Customer — process to verify customer identity before/during relationship | EBA/FATF ✅ |
| CDD | Customer Due Diligence — risk-appropriate verification of customer identity and purpose | EBA ✅ |
| SAR | Suspicious Activity Report — report filed when ML/TF is suspected | FinCEN/regulators ✅ |
| PEP | Politically Exposed Person — elevated-risk customer category | EBA/GL/2024/01 ✅ |
| STR | Suspicious Transaction Report — jurisdiction-specific variant of SAR | Precisa ⚠️ |
| CRA | Customer Risk Assessment — scored rating per customer | Ondato ⚠️ |
| False positive | Flagged transaction that proves legitimate — key efficiency metric | Multiple sources |

---

## Domain & How It Works

AML scoring sits inside the **Anti-Money Laundering (AML) compliance framework** required of banks, payment firms, crypto-asset service providers, and other regulated entities. It operates at two levels:

**1 — Customer Risk Rating (CRA):** Assigns a risk tier (low / medium / high) to each customer at onboarding and on a periodic basis. Inputs include customer type, geography, industry, PEP status, adverse media, and transaction history.

**2 — Transaction-level scoring:** Each transaction (or pattern of transactions) receives a score that triggers review, escalation, or Suspicious Activity Report filing.

Key scoring factors (per Alessa ⚠️ vendor, widely cited):
| Factor | Why it matters |
|--------|---------------|
| Customer type | Corporates vs. individuals carry different risk profiles |
| Geographic risk | High-risk jurisdictions per FATF grey/black lists |
| Transaction amount & frequency | Structuring detection (amounts kept below reporting thresholds) |
| Industry / business type | Some sectors (cash-intensive, DNFBP) are inherently higher risk |
| PEP status | Public officials at elevated bribery/corruption risk |
| Adverse media / sanctions | News hits against customer name |
| Length of relationship | Long-standing = lower default risk |
| Anomalous patterns | Deviations from stated purpose or historical baseline |

Source: Alessa · https://alessa.com/blog/anti-money-laundering-risk-scoring-factors/ ⚠️ vendor

---

## Alternatives / Before

**Before AML scoring — rule-based systems (the predecessor):**
> "Rules-based systems are monitoring frameworks applying fixed thresholds and if-then logic to detect suspicious transactions or customer activity in AML compliance."
Source: Facctum · https://www.facctum.com/terms/rules-based-systems ◽

Limitations of rule-based systems (Facctum ◽):
- Generate excessive false positives (90–99% per Flagright ⚠️ vendor; "up to 95%" per Ondato ⚠️ vendor — both vendor claims, [primary source not verified for this specific figure])
- Cannot detect emerging typologies (cyber-enabled laundering, layering via crypto)
- Static thresholds that don't adapt across jurisdictions

**Evolution path:**
| Generation | Approach | Limitation overcome |
|------------|----------|---------------------|
| Rule-based | Fixed if-then thresholds | — (baseline) |
| Statistical / behavioural | Customer baseline + deviation detection | Reduces some false positives |
| ML/AI-native | Gradient boosting, neural nets, graph analytics | Adapts to new typologies; explainability gap |
| Hybrid | Rules + ML scoring + human-in-loop | Balance compliance explainability with adaptability |

Source: Facctum · https://www.facctum.com/terms/rules-based-systems ◽; Feedzai · https://www.feedzai.com/blog/what-is-aml-transaction-monitoring/ ⚠️ vendor

---

## Market & Players

**Market size:** $33.9 billion (2025) → $75+ billion (2030), 121% growth.
Banks account for 64% of AML spending by 2030.
Source: Juniper Research · https://www.juniperresearch.com/press/aml-systems-market-to-surpass-75-billion-by-2030-globally-with-lexisnexis-risk-solutions-oracle-and-experian-leading-the-defence/ ◽ (press release, Sep 2025)

Transaction monitoring sub-segment: $6.8B by 2028, 17% CAGR.
Source: Feedzai · https://www.feedzai.com/blog/what-is-aml-transaction-monitoring/ ⚠️ vendor — [cross-source corroboration not available for this sub-figure]

| Player | What they do | Positioning |
|--------|-------------|-------------|
| LexisNexis Risk Solutions | Identity, sanctions, adverse media, AML analytics | Incumbent market leader (Juniper #1) ◽ |
| Oracle Financial Services AML | Automated Scenario Calibration, behavioral analytics | Incumbent, bank-grade deployment |
| Experian | End-to-end AML, identity verification | Incumbent (Juniper top 3) |
| NICE Actimize | AI/ML financial crime prevention, case management | Legacy incumbent, broad feature set |
| SAS Institute | Predictive modeling, network analysis, AML | Legacy, analytics heritage |
| ComplyAdvantage | Real-time risk database, AI-driven pattern recognition | Modern challenger, risk data focus |
| Feedzai | RiskOps platform, real-time AI scoring | Modern challenger, payments native |
| Flagright | No-code, AI-native, explainable scoring | New entrant, compliance-team UX focus |
| Quantexa | Graph analytics, network intelligence | Differentiated on entity resolution |
| Nasdaq Verafin | Cloud AML, cross-institution fraud consortium | Niche: consortium data |

Sources: Juniper Research ◽; ComplyCube guide · https://www.complycube.com/en/top-10-aml-software-for-banks-buyers-guide/ ⚠️; Flagright · https://www.flagright.com/post/top-aml-risk-scoring-software ⚠️

---

## Regulation & Standards

| Body | Document | Year | What it requires |
|------|----------|------|-----------------|
| FATF ✅ | 40 Recommendations (updated 2023) | 2003→2023 | Risk-based approach to AML/CFT; customer risk assessment mandatory | [page 403 — referenced via multiple independent sources] |
| EBA ✅ | EBA/GL/2024/01 — Amending Guidelines on ML/TF Risk Factors | 2024 | Extended sector coverage (CASPs, crowdfunding, PISPs, AISPs, currency exchange); application deadline Dec 30, 2024 · https://www.eba.europa.eu/legacy/regulation-and-policy/regulatory-activities/anti-money-laundering-and-countering-financing-1 |
| EU ✅ | 6th Anti-Money Laundering Directive (AMLD6) + AMLR | 2021→2024 | Harmonised EU AML rulebook; AMLA (new EU AML authority) mandate ongoing |
| FinCEN ✅ | BSA/AML Program requirements | ongoing | Documented risk assessment required of US financial institutions |
| Wolfsberg Group ◽ | Statement on Effective Monitoring for Suspicious Activity | 2019 | Industry standard: monitoring "should not exist in isolation but form part of a broader MSA program" · https://www.napier.ai/post/transaction-monitoring-screening-aml |

---

## Adjacent Concepts

| Concept | Distinction from AML scoring | Source |
|---------|------------------------------|--------|
| Transaction screening | Pre-transaction, real-time check against *sanctions lists / PEP watchlists* — answers "is this entity sanctioned?"; not a risk score | Napier AI ◽ |
| Transaction monitoring | Retrospective/real-time *behavioral pattern* analysis across transaction history — feeds the score; not the same as the score itself | Napier AI ◽ https://www.napier.ai/post/transaction-monitoring-screening-aml |
| KYC | Identity verification process — the input data for CDD and scoring; precedes scoring | EBA ✅ |
| Fraud scoring | Scores transaction legitimacy (account takeover, card fraud) — overlapping tech, different predicate | Multiple |
| Credit scoring | Scores repayment probability — different risk domain, similar numeric output | — |
| SAR / STR | The *output* action when a score exceeds threshold — not the score itself | Regulators ✅ |

---

## Artefacts

| Type | Resource | Link | Status |
|------|----------|------|--------|
| Reference glossary | ACAMS Glossary of Terms | https://www.acams.org/en/resources/aml-glossary-of-terms | ✅ 200 OK |
| Official guidance (web) | EBA ML/TF Risk Factors Guidelines portal | https://www.eba.europa.eu/legacy/regulation-and-policy/regulatory-activities/anti-money-laundering-and-countering-financing-1 | ✅ 200 OK |
| Working paper | BIS Bulletin #111 — AML compliance for cryptoassets (scoring model example) | https://www.bis.org/publ/bisbull111.htm | ✅ 200 OK |
| Buyers guide | ComplyCube Top 10 AML Software for Banks | https://www.complycube.com/en/top-10-aml-software-for-banks-buyers-guide/ | ✅ 200 OK |
| Live demo / sandbox | not found — no public AML scoring sandbox identified in this run | — | — |

---

## Research Snapshot

Direct academic fetch blocked (ResearchGate 403). Below from search snippets only — treat as leads, not verified paper content.

| Paper / Source | Year | Key finding (from search snippet) | Epistemic class |
|---------------|------|-----------------------------------|-----------------|
| "Rule-Based Systems in AML" (ResearchGate) | ~2024–25 | Comparative analysis of rule-based limitations — title only, not fetchable | ⚠️ [abstract only — source 403] |
| "Comparative Performance: Rule-Based vs. ML in Crypto AML" (ResearchGate) | ~2024–25 | ML outperforms rule-based in crypto environments — title only | ⚠️ [abstract only — source 403] |
| BIS Bulletin #111 — "AML compliance for cryptoassets" | 2023 | Proposes a blockchain-based AML scoring (0–100 scale); "higher scores denote clean funds from allow-listed wallets" | ◽ working paper |

**Consensus:** Emerging — active academic and industry research, no single definitive standard; shift from rule-based to ML-hybrid is consensus direction among practitioners but no regulatory mandate for specific scoring methodology exists. "Explainability" is the contested frontier.

Source: BIS · https://www.bis.org/publ/bisbull111.htm ◽; Juniper (quote: "vendors must deliver intelligent, adaptable systems") ◽

---

## Next: Explore

- **Transaction monitoring** — the behavioral analysis layer that generates the data AML scoring aggregates; a snapshot here would map the detection typologies and the alert management lifecycle
- **Customer Due Diligence (CDD) / Enhanced Due Diligence (EDD)** — the regulatory process that consumes AML risk scores and determines customer relationship outcomes
- **KYC / Identity verification** — the upstream data collection process; quality of identity data directly constrains scoring accuracy
- **FATF 40 Recommendations** — the global standard that mandates risk-based approaches; understanding it clarifies why scoring exists at all
- **Financial crime graph analytics** — emerging approach (Quantexa, Feedzai IQ) using network intelligence to detect layering and smurfing patterns invisible to individual transaction scoring

*(Run `/penguru:snapshot <concept>` on any of these)*

---

## Sources

| # | Title | URL | Type | Status |
|---|-------|-----|------|--------|
| 1 | AML Network — Risk Score Glossary | https://amlnetwork.org/aml-glossary/risk-score/ | ◽ independent | ✅ live |
| 2 | EBA — ML/TF Risk Factors Guidelines portal | https://www.eba.europa.eu/legacy/regulation-and-policy/regulatory-activities/anti-money-laundering-and-countering-financing-1 | ✅ official | ✅ live |
| 3 | BIS Bulletin #111 — AML for cryptoassets | https://www.bis.org/publ/bisbull111.htm | ✅ official | ✅ live |
| 4 | Alessa — AML Risk Scoring Factors | https://alessa.com/blog/anti-money-laundering-risk-scoring-factors/ | ⚠️ vendor | ✅ live |
| 5 | Juniper Research — AML Market Forecast press release | https://www.juniperresearch.com/press/aml-systems-market-to-surpass-75-billion-by-2030-globally-with-lexisnexis-risk-solutions-oracle-and-experian-leading-the-defence/ | ◽ analyst | ✅ live |
| 6 | Napier AI — Transaction Monitoring vs Screening | https://www.napier.ai/post/transaction-monitoring-screening-aml | ◽ independent | ✅ live |
| 7 | Facctum — Rules-Based Systems definition | https://www.facctum.com/terms/rules-based-systems | ◽ independent | ✅ live |
| 8 | Precisa — AML Risk Scoring mechanics | https://precisa.in/blog/aml-risk-scoring-suspicious-transaction-monitoring/ | ⚠️ vendor | ✅ live |
| 9 | Feedzai — AML Transaction Monitoring | https://www.feedzai.com/blog/what-is-aml-transaction-monitoring/ | ⚠️ vendor | ✅ live |
| 10 | Ondato — AML Risk Assessment | https://ondato.com/blog/aml-risk-assessment/ | ⚠️ vendor | ✅ live |
| 11 | ComplyCube — Top 10 AML Software guide | https://www.complycube.com/en/top-10-aml-software-for-banks-buyers-guide/ | ⚠️ vendor | ✅ live |
| 12 | Flagright — Top AML Risk Scoring Software | https://www.flagright.com/post/top-aml-risk-scoring-software | ⚠️ vendor | ✅ live |
| 13 | ACAMS — Glossary of Terms | https://www.acams.org/en/resources/aml-glossary-of-terms | ◽ independent | ✅ live |

**FATF pages:** Referenced in multiple independent sources; direct URL returns 403 (government bot-blocking). Cited as ✅ but not directly fetched.
**ResearchGate papers:** Found in search, blocked (403) — not fetched, not cited as content.

*Dimensions with "not found": live AML scoring sandbox/demo · academic papers (direct fetch blocked)*
*Dead links dropped from original shortlist: FATF .org pages (403), FFIEC (403), ResearchGate (403), FATF PDF (403)*
