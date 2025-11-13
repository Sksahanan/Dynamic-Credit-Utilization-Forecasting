# Dynamic Credit Utilization Forecasting  
### Predictive & Prescriptive Customer Segmentation (Deloitte – Analytics Capstone)

**Role:** Project Manager & Analytics Lead  
**Team:** Group 3, PGDM (Business Analytics) 2024–2026 – JAGSoM  

This repository contains the complete workflow for a **credit card portfolio analytics project** that I led as Project Manager.  
The goal is to help a bank / card issuer **decide smarter credit limits and APRs** using data instead of static rules. :contentReference[oaicite:0]{index=0}  

---

## 1. Business Problem & Context

Credit card usage is growing fast, but so is **credit risk**:

- Traditional policies use static rules (income, bureau score, city), applied mostly at card issuance.
- These rules **ignore how customer behaviour changes over time** – spending can rise, repayment discipline can fall, and macro conditions can deteriorate. :contentReference[oaicite:1]{index=1}  
- As a result:
  - Safe customers are **under-served** (low limits, generic offers).
  - Risky customers are **over-exposed** (high limits, high probability of default).
  - Portfolios suffer from **hidden risk and missed revenue**.

As Project Manager, I framed the key problem as:

> “How can we dynamically match credit policy (limits & APR) to actual customer behavior,  
> balancing **growth** with **risk control** in a way that is explainable to business teams?” :contentReference[oaicite:2]{index=2}  

---

## 2. Project Objectives

We defined three clear, measurable objectives for the project. :contentReference[oaicite:3]{index=3}  

1. **Segment customers into meaningful groups**  
   - Based on **utilization**, **repayment discipline**, and **cash-advance dependency**.  
   - Create clear personas: *Safe High Spenders*, *Moderate Users*, *Revolvers*, *Over-Leveraged*.

2. **Forecast future credit utilization under stress**  
   - Simulate “what-if” scenarios such as **+200 bps interest rate hike** and **–15% drop in repayment discipline**. :contentReference[oaicite:4]{index=4}  
   - Estimate how balances, utilization, and risk change by persona and geography.

3. **Recommend personalized credit strategies**  
   - Translate signals into **dynamic credit limit** and **APR recommendations** per customer.
   - Produce **ops-ready tables** for CRM and credit operations (who to upgrade, who to contain).

---

## 3. Data & Sources

We combined **two datasets**, using `CUST_ID` as the key. :contentReference[oaicite:5]{index=5}  

| Dataset                       | Description                                                                 |
|------------------------------|-----------------------------------------------------------------------------|
| `cc_general.csv`             | Historical credit card behavior – balances, credit limits, purchases, payments, cash advances, full-payment %, tenure, and frequencies. |
| `credit_card_demographics.csv` | Customer demographics – age, gender, income, occupation, marital status, education, city tier, region, dependents, home ownership, etc. :contentReference[oaicite:6]{index=6} |

As PM, I defined the **data preparation scope** as:

- Build a **single, clean data foundation** by merging transactional and demographic data.
- Standardize numeric types, handle missing values, and cap extreme outliers.
- Create **behaviour features**:
  - `Utilization Ratio = BALANCE / CREDIT_LIMIT`
  - `Repayment Discipline = PRC_FULL_PAYMENT + (PAYMENTS vs MINIMUM_PAYMENTS)`
  - `Cash Advance Dependency = CASH_ADVANCE / PURCHASES` :contentReference[oaicite:7]{index=7}  

---

## 4. Solution Design (PM View)

We used a **three-layer analytics framework** that mirrors how banks think: :contentReference[oaicite:8]{index=8}  

1. **Descriptive – “What is happening?”**  
   - Build simple metrics (Utilization, Repayment Discipline, Cash-Advance Dependency).  
   - Combine with demographics (age, income band, city tier, region, tenure).  
   - Create **clear personas**: Safe High Spenders, Moderate Users, Revolvers, Over-Leveraged.

2. **Predictive – “What may happen next?”**  
   - Run **stress tests**: e.g., +200 bps APR, –15% full-pay behaviour.  
   - Estimate impact on balances, utilization, and risk at **customer, segment, and region** levels.

3. **Prescriptive – “What should we do?”**  
   - Design a **policy engine** that links signals → actions:
     - Increase limits / lower APR for disciplined, under-utilized customers.
     - Hold or reduce limits for over-leveraged or deteriorating profiles.
     - Offer education / payment plans where behavior indicates temporary distress.
   - Ensure rules are **simple, explainable, and auditable** for regulators and internal governance. :contentReference[oaicite:9]{index=9}  

---

## 5. Architecture & Tech Stack

### 5.1 Data Architecture (Bronze → Silver → Gold → KPIs)

As PM, I aligned the team on a **modern lakehouse pattern**:

- **Bronze Layer** – Raw views: `cc_general_bronze`, `cc_demo_bronze` :contentReference[oaicite:10]{index=10}  
- **Silver Layer** – Cleaned & typed tables: `cc_general_silver`, `cc_demo_silver`
- **Gold Layer** – Feature & modeling tables: `cc_features_gold`, `cc_segments`, `cc_util_forecast`
- **KPI / Ops Layer** – Business outputs:  
  - `cc_business_impact`, `target_limit_upgrades`, `apr_ladder_summary`, `risk_heatmap_region_tier`, etc.

This structure makes the project **modular, scalable, and production-friendly**.

### 5.2 Tools & Technologies

- **Databricks SQL (Unity Catalog)** – data engineering pipeline, feature creation, and analytics runbook. :contentReference[oaicite:11]{index=11}  
- **Delta Tables / Views** – for Bronze, Silver, Gold, and KPI layers.
- **SQL-only solution** for this phase (ML models planned in future scope).

---

## 6. Key Features in This Repo

From a Project Manager perspective, the repository is organized around **clear deliverables**: :contentReference[oaicite:12]{index=12}  

- `1.Project Scope Document.pdf`  
  – Business goals, in-scope & out-of-scope items, KPIs, end goal.

- `2.SQL Query to build the Table Dynamic Credit Utilization Forecasting.pdf`  
  – Full Databricks SQL **runbook**:  
    - Building Bronze → Silver → Gold tables.  
    - Creating segmentation, stress-test, and prescription views. :contentReference[oaicite:13]{index=13}  

- `3.Query Table And Result- Dynamic Credit Utilization Forecasting.pdf`  
  – Query outputs and **interpretation tables** for each objective.

- `4.1.Conclusion And Recommendation -Dynamic Credit Utilization Forecasting.pdf`  
  – **Executive summary, findings, and recommendations** for business stakeholders. :contentReference[oaicite:14]{index=14}  

---

## 7. Results – What We Found

### 7.1 Objective 1 – Segmentation

- The portfolio is dominated by **Moderate Users** and **Revolvers**, while **Safe High Spenders** are a smaller but very valuable group.
- Behaviour by persona:
  - **Safe High Spenders** → very low utilization, very high full-payment discipline.
  - **Moderate Users** → mid-level utilization, some full-payment behaviour.
  - **Revolvers** → high utilization, almost never pay in full.

**Project Manager takeaway:**  
The segments are **clear, intuitive, and business-friendly**, which makes it easier for Risk, Marketing, and Operations to use them directly in policy and campaign decisions.

---

### 7.2 Objective 2 – Stress Forecasting

Stress scenario used (configurable via parameters):

- **+200 bps** increase in APR.
- **15% drop** in full-payment behaviour.

Key findings:

- Average utilization jumps significantly under stress, indicating that a mild macro shock can push many customers closer to their limits.
- The risk score also increases, with **Revolvers** becoming extremely risky (often behaving as over-limit customers under stress).
- **Risk clusters** appear in multiple geographies (some rural/semi-urban as well as metro pockets), so vulnerability is **not limited to one region**.

**Project Manager takeaway:**  
We converted “hidden future risk” into **visible, quantified stress** at segment and region level, enabling the creation of **early-warning watchlists** and regional steering strategies.

---

### 7.3 Objective 3 – Prescriptive Strategy

From the full portfolio:

- A majority of customers fall into a **“No Change”** bucket for policy.
- A meaningful share is flagged for **limit reductions** due to high risk.
- A smaller but high-potential subset is flagged for **limit upgrades** with relatively low risk.

APR ladder analysis shows:

- Safe High Spenders tend to receive **APR reductions** (rewarding good behaviour).
- Risky Revolvers tend to receive **APR increases** or tighter credit terms.

**Project Manager takeaway:**  
Actions are **targeted, explainable, and aligned with risk-based pricing principles**. For every recommendation (upgrade, hold, or reduce), there is a clear, data-driven rationale that can be explained to business stakeholders and regulators.

---

## 8. What This Shows About My Role as Project Manager

In this project, my responsibilities went beyond writing SQL. I acted as the **bridge between business and analytics**:

- **Defining the scope & objectives**  
  - Framed the problem around three pillars: segmentation, stress forecasting, and personalized credit strategy.
  - Clarified what is in-scope (data pipeline, SQL analytics, prescriptive rules) and what is out-of-scope for this phase (real-time deployment, advanced ML models).

- **Designing the analytics framework**  
  - Structured the work as **Descriptive → Predictive → Prescriptive**, so stakeholders can follow the logic from raw behaviour to final decisions.
  - Ensured every technical component (tables, views, KPIs) is tied to a clear business question.

- **Coordinating data & engineering work**  
  - Aligned the team on using a **Bronze → Silver → Gold → KPI** data pipeline.
  - Made sure data was consistently cleaned, joined, and versioned so analyses are reproducible and auditable.

- **Translating outputs into business language**  
  - Converted query outputs into **insights, recommendations, and policy actions** that non-technical stakeholders can understand.
  - Highlighted trade-offs between growth and risk, and provided concrete levers (limit changes, APR moves, regional strategies).

- **Focusing on governance & fairness**  
  - Included fairness checks (e.g., APR ladder by income bands and segments).
  - Kept all scenario and economic assumptions in parameter tables so they are **transparent and easy to review**.

Overall, this repo is not just a collection of queries; it is a **managed analytics project** with clear goals, structure, and decision pathways.

---

## 9. Limitations & Future Scope

### 9.1 Limitations

- The analysis is based on **historical, static data** (snapshot), not real-time streaming transactions.
- Segmentation and stress tests are performed on a single time frame; they do not yet capture **customer movement between segments over time**.
- The stress scenario is limited to a single configuration (e.g., +200 bps, –15% full-pay). In practice, institutions run multiple scenarios (mild, moderate, severe).
- No external **credit bureau data** (e.g., CIBIL/Experian scores) or income verification sources are integrated in this phase.
- Recommendations are **rule-based** (SQL logic), not yet supported by machine learning models that could learn complex patterns.

### 9.2 Future Scope

- **Add machine learning models**  
  - Unsupervised models (e.g., K-Means clustering) to discover deeper micro-segments.  
  - Supervised models to predict default risk, response to limit upgrades, and churn/lifetime value.

- **Introduce time-series and behavioural tracking**  
  - Move from snapshot analysis to **longitudinal tracking** of how utilization, payments, and risk evolve over time.  
  - Enable dynamic movement between segments.

- **Enhance stress testing and scenario design**  
  - Support multiple interest-rate and macroeconomic scenarios (mild / moderate / severe).  
  - Integrate macro indicators as external drivers.

- **Integrate external data sources**  
  - Credit bureau scores, income verification, employment stability, etc.  
  - Enrich demographics with geospatial and socio-economic indicators.

- **Operational deployment**  
  - Wrap the results into dashboards and APIs for credit/risk teams.  
  - Automate periodic refresh (monthly/quarterly) and embed approvals and governance workflows.

These extensions would transform the project from a strong **data-driven prototype** into a **production-grade decision support system** for credit risk and product teams.
