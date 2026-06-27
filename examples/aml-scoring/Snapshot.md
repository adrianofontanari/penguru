---
marp: true
theme: penguru
paginate: true
---

# AML Scoring
*2026-06-27 · Technology/Practice · 13 sources · light mode*

> "A numerical or categorical value that quantifies the money laundering and terrorist financing risk posed by a customer, transaction, product, or geographic area."

Trust legend: ✅ official · ◽ independent · ⚠️ vendor · 💬 KOL opinion

*Source: AML Network ◽ · https://amlnetwork.org/aml-glossary/risk-score/*

---

## What it is

**AML scoring** assigns a numeric or tiered risk level to customers and transactions to detect money laundering / terrorist financing.

Two kinds:
- **Customer Risk Rating (CRA):** risk tier at onboarding + periodic review
- **Transaction score:** per-event score triggering review, escalation, or SAR filing

**Primary source:** AML Network ◽ · https://amlnetwork.org/aml-glossary/risk-score/

---

## Glossary + Don't confuse with

| Term | Definition |
|------|------------|
| KYC | Verify customer identity — *input* to scoring |
| CDD | Customer Due Diligence — *process* consuming the score |
| SAR / STR | Suspicious report filed when score exceeds threshold — *output* |
| PEP | Politically Exposed Person — high-risk customer category |
| ML/TF | Money Laundering / Terrorist Financing |

**Don't confuse AML scoring with:**
- **Transaction screening** → real-time sanctions-list check, not a pattern score ◽
- **Credit scoring** → repayment risk, different domain entirely

Source: EBA/GL/2024/01 ✅; Napier AI ◽ · https://www.napier.ai/post/transaction-monitoring-screening-aml

---

## Where it lives + How it works

AML scoring is embedded in regulated financial services: banks, payment firms, crypto-asset service providers, money transfer operators.

**How it scores:**

| Factor | Why it matters |
|--------|---------------|
| Customer type | Corporates vs. individuals: different baseline risk |
| Geographic risk | FATF grey/blacklist jurisdictions ✅ |
| PEP status | Public officials → elevated corruption risk |
| Transaction patterns | Structuring, rapid cycling, mule indicators |
| Adverse media / sanctions | Real-time news hits |

Source: Alessa ⚠️ · https://alessa.com/blog/anti-money-laundering-risk-scoring-factors/

---

## Alternatives / Before

**Before:** Rule-based systems — fixed thresholds and if-then logic.

> "Financial crime has evolved beyond simple patterns" making "static rules insufficient."
Source: Facctum ◽ · https://www.facctum.com/terms/rules-based-systems

| Generation | Approach | Key problem |
|------------|----------|-------------|
| Rule-based | Fixed thresholds | 90–99% false positives ⚠️ |
| Statistical/behavioural | Customer baseline | Partial improvement |
| ML/AI-native | Gradient boosting, graph analytics | Explainability gap |
| Hybrid | Rules + ML + human review | Current best practice |

False-positive figures: ⚠️ vendor claim — [primary source not independently verified]

---

## Rules

| Body | Document | Year | What it requires |
|------|----------|------|-----------------|
| FATF ✅ | 40 Recommendations | 2003→2023 | Risk-based approach mandatory; customer risk assessment required |
| EBA ✅ | EBA/GL/2024/01 | 2024 | Extended ML/TF risk factor guidelines; deadline Dec 30, 2024 |
| EU ✅ | AMLD6 + AMLR | 2021→2024 | Harmonised EU rulebook; new AMLA authority |
| FinCEN ✅ | BSA/AML Program | ongoing | Documented risk assessment required for US institutions |

*FATF pages returned 403 — referenced via multiple independent sources*
Source: EBA portal ✅ · https://www.eba.europa.eu/legacy/regulation-and-policy/regulatory-activities/anti-money-laundering-and-countering-financing-1

---

## Market & players

**Market:** $33.9B (2025) → $75B+ (2030), 121% growth · 64% = banks
Source: Juniper Research ◽ · https://www.juniperresearch.com/press/aml-systems-market-to-surpass-75-billion-by-2030-globally-with-lexisnexis-risk-solutions-oracle-and-experian-leading-the-defence/

| Player | What they do | Tag |
|--------|-------------|-----|
| LexisNexis Risk Solutions | Identity, sanctions, AML analytics | ⚠️ Juniper #1 ◽ |
| Oracle / NICE Actimize | Enterprise AML suites, case management | ⚠️ incumbents |
| ComplyAdvantage | Real-time risk database, AI scoring | ⚠️ modern challenger |
| Feedzai / Flagright | AI-native, real-time transaction risk | ⚠️ new entrants |
| Quantexa | Graph analytics / entity resolution | ⚠️ differentiated |

---

## What to touch

| Type | Resource | Link |
|------|----------|------|
| Glossary | ACAMS — AML Glossary of Terms | https://www.acams.org/en/resources/aml-glossary-of-terms |
| Official guidance | EBA ML/TF Risk Factors portal | https://www.eba.europa.eu/legacy/regulation-and-policy/regulatory-activities/anti-money-laundering-and-countering-financing-1 |
| Working paper | BIS Bulletin #111 — AML scoring for crypto | https://www.bis.org/publ/bisbull111.htm |
| Buyers guide | ComplyCube Top 10 AML Software | https://www.complycube.com/en/top-10-aml-software-for-banks-buyers-guide/ |

*Live sandbox/demo: **not found** in this run*

All links validated ✅ (HTTP 200)

---

## What research says

| Source | Type | Key finding |
|--------|------|-------------|
| BIS Bulletin #111 (2023) ◽ | Working paper | Proposes 0–100 AML score for crypto; "higher scores = clean funds from allow-listed wallets" |
| ResearchGate papers found in search | — | Rule-based vs. ML comparative research — ⚠️ [pages blocked 403, not fetchable] |

**Consensus: Emerging.** Practitioner shift from rule-based to ML-hybrid is near-unanimous. No regulatory mandate for a specific scoring method. "Explainability" is the contested frontier — regulators demand it, black-box ML struggles with it.

Source: BIS ✅ · https://www.bis.org/publ/bisbull111.htm; Juniper ◽

---

## Next: explore

- **Transaction monitoring** — the pattern-detection layer that feeds AML scores; snapshot it to map alert typologies and lifecycle
- **Customer Due Diligence (CDD/EDD)** — the process that consumes scores; maps to FATF Rec 10
- **KYC / Identity verification** — upstream data quality; constrains scoring accuracy
- **FATF 40 Recommendations** — the global standard mandating risk-based AML approaches
- **Financial crime graph analytics** — network intelligence (entity resolution, layering detection) beyond transaction-level scoring

*Run `/penguru:snapshot <concept>` on any of these*

---

## Sources & trust audit

| Type | Count | Key examples |
|------|-------|-------------|
| ✅ Official / regulator | 4 | EBA, FATF, BIS, FinCEN |
| ◽ Independent practitioner | 5 | AML Network, Juniper Research, Napier AI, Facctum, BIS working paper |
| ⚠️ Vendor | 6 | Alessa, Ondato, Feedzai, Flagright, Precisa, ComplyCube |
| 💬 KOL opinion | 0 | — |
| **Total** | **13** | |

**Gaps ("not found"):** live AML scoring sandbox/demo · academic papers (ResearchGate 403)

**Dead links dropped:** FATF .org pages (403) · FFIEC BSA/AML (403) · ResearchGate papers (403) · FATF Risk-Based Approach PDF (403)

**⚠️ Caveat:** false-positive percentages (90–99%) are vendor claims — [primary academic source not verified]
