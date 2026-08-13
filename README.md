# Victor Nguyen — Engineering Portfolio

Computer Science | SWE | Data & AI Systems

---

This repository organizes supporting evidence for selected software and data projects.

The goal is to make project scope, personal contribution, technical decisions, testing, and measurable results easier to verify.

> Status: Work in progress  
> Last updated: 2026-08-11

---

## Selected Work

### 1. Laboratory Information Management System

Software used to support environmental field and laboratory workflows for a 10–15-person research group.

**Why it matters:** real users, workflow translation, validation before database writes, desktop + Android delivery, Firebase-backed data management, and CSV export for downstream analysis.

**Scale:** 1,200+ physical samples and 4,000+ associated laboratory records. A physical sample can produce multiple water, sediment, identity, and DNA records.

- [Project Summary](./lims/PROJECT-SUMMARY.md)
- [Engineering RFC](./lims/RFC-SUMMARY.md)
- [Evidence Index](./lims/evidence-links.md)

**Source:** private because the application and underlying research data include confidential laboratory information. Public evidence in this repository is limited to sanitized or synthetic material.

**Current engineering focus:**

- automated validation and workflow tests;
- synthetic fixtures for public verification;
- authorization/security-rule testing;
- dependency-failure behavior;
- reproducible public-safe demo evidence.

### 2. NYC Taxi Analytics Warehouse & Operations Dashboard

A Python, SQL, DuckDB, Parquet, Streamlit, and scikit-learn project for processing and analyzing tens of millions of NYC Yellow Taxi trip records.

**Why it matters:** large-scale local analytics, raw/clean/rejected/mart separation, SQL validation, dashboard-oriented data modeling, performance reasoning, and an exploratory regression evaluation.

**Scale:** approximately 48M+ 2025 taxi trip records in the large-scale workflow.

- [Project Summary](./NYCtaxi/PROJECT-SUMMARY.md)
- [Engineering RFC](./NYCtaxi/RFC-SUMMARY.md)
- [Evidence Index](./NYCtaxi/evidence-links.md)
- [Source Repository](https://github.com/vodliosje/TaxiNYC)

**Current engineering focus:**

- automated data-quality tests;
- deterministic two-run/idempotency checks;
- clean-environment reproducibility;
- query/runtime benchmarks;
- reproducible ML split, leakage audit, and evaluation.

### 3. PlantSight

A three-person sustainability hackathon web application using JavaScript, Firebase, and Google Maps.

**Why it matters:** full-stack delivery under time constraints, authentication, Firebase-backed state, rating/scoring logic, map integration, and team ownership boundaries.

OpenWeather and Perenual were considered during design but were **not implemented in the completed hackathon prototype**.

- [Project Summary](./plantsight/PROJECT-SUMMARY.md)
- [Engineering RFC](./plantsight/RFC-SUMMARY.md)
- [Evidence Index](./plantsight/evidence-links.md)
- [Source Repository](https://github.com/vodliosje/VieGanG-geaux)

**Current engineering focus:**

- reproducible setup;
- rating/authentication tests;
- Firebase authorization review;
- concurrent score-update behavior;
- Maps/Firebase failure states;
- teammate ownership confirmation.

## Portfolio Positioning

The projects are intentionally complementary:

| Project    | Primary signal                      | Secondary signal                           |
| ---------- | ----------------------------------- | ------------------------------------------ |
| LIMS       | software engineering for real users | data validation, security, workflow design |
| NYC Taxi   | backend/data systems at scale       | SQL, performance, ML evaluation            |
| PlantSight | full-stack and team delivery        | APIs, authentication, failure handling     |

For general software-engineering internship preparation, the improvement priority is:

1. **LIMS** — strongest real-world ownership; deepen correctness, testing, and reliability evidence.
2. **NYC Taxi** — strongest systems/data-scale project; deepen reproducibility, idempotency, performance, and ML methodology.
3. **PlantSight** — preserve as full-stack/team breadth; harden only the most important security and failure paths.

## Evidence Principles

1. Claims should be supported by inspectable or reproducible evidence where possible.
2. Team contributions must not be presented as individual work.
3. Private research data, credentials, secrets, and sensitive user information must not be published.
4. Quantitative claims should have a reproducible source or conservative wording.
5. Unsupported claims are removed instead of reverse-engineered to match a resume statement.
6. Planned work is labeled as planned; it is not presented as implemented work.

## Evidence and Ownership Inventory

See [ASSET-AND-OWNERSHIP-INVENTORY.md](./ASSET-AND-OWNERSHIP-INVENTORY.md).
