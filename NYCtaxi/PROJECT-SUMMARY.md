# NYC Taxi Analytics Warehouse & Operations Dashboard

## 1. Problem

This project uses NYC Yellow Taxi trip data to build a reproducible analytics workflow for questions about demand, revenue, fare behavior, duration, tipping, geographic activity, data quality, and anomalous trips.

The project intentionally goes beyond a notebook-only analysis. Tens of millions of rows require explicit data layers, repeatable transformations, validation, and dashboard-oriented aggregates.

## 2. Project Context

The system models an operations-analytics use case for mobility analysts or data stakeholders who need to answer questions such as:

- when demand is highest;
- which zones generate the most trips or revenue;
- how fares relate to distance and duration;
- which records are invalid or suspicious;
- how tipping changes by time, zone, and payment type;
- whether a simple regression model can provide a useful fare baseline.

The data source is the public NYC Taxi & Limousine Commission trip-record dataset.

## 3. What I Built

I designed and implemented the full project.

The workflow evolved in three stages:

1. a Pandas prototype on a smaller period to understand schema and cleaning behavior;
2. a DuckDB/SQL warehouse-style implementation;
3. a larger multi-month/full-year workflow processing approximately 48M+ 2025 trip records.

Main components include:

- Parquet raw-data layer;
- DuckDB database and raw trip view/table;
- clean and rejected trip tables;
- derived trip features;
- analytical mart tables;
- SQL data-quality checks;
- EDA notebooks;
- Streamlit operations dashboard;
- exploratory scikit-learn regression evaluation.

## 4. Ownership

I own the project design and implementation, including:

- data acquisition/organization;
- Pandas prototype;
- cleaning and rejection rules;
- feature derivation;
- DuckDB/SQL transformations;
- mart design;
- SQL validation;
- dashboard-ready outputs;
- Streamlit dashboard;
- regression baseline/model evaluation and error analysis.

## 5. Architecture

```text
NYC TLC Parquet files
        ↓
Raw layer / DuckDB raw view
        ↓
Profiling + validity rules
        ↓
Clean trips + rejected trips with reasons
        ↓
Derived features
        ↓
Analytical marts
        ↓
SQL/EDA ───── Streamlit dashboard
        ↓
Modeling dataset
        ↓
Regression evaluation
```

Representative marts include:

- hourly demand;
- daily/monthly revenue;
- pickup/dropoff zone demand;
- payment/tip behavior;
- anomaly summaries.

The raw source remains unchanged. Normal business metrics are built from cleaned data, while rejected records remain available for audit and data-quality analysis.

## 6. Key Engineering Decisions

### DuckDB + Parquet for local analytical scale

DuckDB can query Parquet efficiently and supports SQL transformations without requiring a separate database server. This made it appropriate for a portfolio system that needs tens-of-millions-of-row analytics while remaining reproducible on a developer machine.

### Pandas for exploration, SQL for repeatable transformation

Pandas was useful for early schema inspection, feature exploration, and modeling samples. DuckDB/SQL became the primary path for scalable cleaning, validation, aggregation, and marts.

### Separate raw, clean, rejected, and mart layers

The project does not silently delete invalid rows. Rejected records are preserved with reasons. Marts provide smaller, stable interfaces for dashboard queries instead of repeatedly scanning raw data.

### Treat ML as an evaluated system component, not a separate demo

The regression work is a bridge from analytics to ML methodology. The important evidence is not the presence of scikit-learn; it is the ability to reproduce a split, compare against a baseline, check leakage, and explain error patterns.

## 7. Data Quality and Reliability

Representative checks include:

- missing pickup/dropoff timestamps;
- dropoff at or before pickup;
- non-positive or extreme durations;
- non-positive or extreme trip distances;
- negative fare/total/tip values;
- passenger counts outside expected bounds;
- missing pickup/dropoff locations;
- suspicious/impossible speed;
- exact and trip-like duplicates.

The intended invariants are:

```text
raw input remains unchanged
clean rows pass core validity rules
rejected rows preserve invalid records + reasons
marts derive from clean data
rerunning the same input should reproduce the same outputs
```

At present, many checks are SQL/notebook based. The next step is to convert representative invariants into automated fixture-based tests.

## 8. Scale and Results

A large 2025 run processes approximately **48M+ taxi trip records**. The conservative resume claim should remain 48M+ unless a saved reproducible count is used to support a more precise number.

The current exploratory regression work compares a mean baseline against Linear Regression. One stronger experiment reported approximately:

```text
Baseline MAE:  $11.941
Baseline RMSE: $18.176
Baseline R²:   ~0.000

Linear Regression MAE:  $3.123
Linear Regression RMSE: $6.002
Linear Regression R²:   0.891
```

A weaker experiment produced much lower explanatory power, which was useful because it exposed sensitivity to feature preparation and modeling setup.

These metrics should be treated as **exploratory evidence until the train/test split, feature availability at prediction time, and leakage checks are made reproducible**. The strongest ML claim is therefore the evaluation process, not a production-quality fare predictor.

## 9. Current Limitations

- automated test coverage is incomplete;
- idempotent two-run behavior is documented as an expectation but not yet formally proven;
- some paths/setup may still depend on the local project structure;
- runtime, peak memory, storage, and query benchmarks need a reproducible baseline;
- dashboard queries should be audited to ensure marts are used for scale-sensitive pages;
- regression evaluation needs a deterministic split and explicit leakage audit;
- Linear Regression is intentionally a simple baseline and will not capture all fare regimes or nonlinear behavior.

## 10. Strongest Evidence to Build Next

1. Tiny deterministic fixture that runs raw → clean/rejected → marts.
2. Automated row-reconciliation and data-quality tests.
3. Two-run equality/idempotency test.
4. Hand-computed mart aggregate test.
5. Clean-environment run path + dependency lock + CI.
6. Query-plan and raw-vs-mart timing comparison.
7. Full/large-run runtime, memory/storage benchmark.
8. Deterministic ML evaluation script with leakage audit and segment error analysis.

## 11. Source and Evidence

- Source repository: https://github.com/vodliosje/TaxiNYC
- [Evidence Index](./evidence-links.md)
- [Engineering RFC](./RFC-SUMMARY.md)
