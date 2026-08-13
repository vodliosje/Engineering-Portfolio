# Engineering RFC — NYC Taxi Reproducibility and Correctness Floor

## Decision

Keep the DuckDB/Parquet/SQL architecture and add a **small deterministic verification path** that proves pipeline correctness without requiring CI or reviewers to process the full 48M+ dataset.

## Goals

- make the pipeline runnable from a clean environment;
- prove raw → clean/rejected → mart transformations on a small fixture;
- prove same-input reruns are deterministic/idempotent;
- establish performance evidence separately from correctness evidence;
- make regression evaluation reproducible and leakage-aware.

## Non-Goals

- replacing DuckDB with a cloud warehouse for resume value;
- introducing Spark/Hadoop without a demonstrated need;
- optimizing before measuring;
- chasing a higher ML metric without establishing a valid evaluation protocol.

## Why Keep DuckDB

The project needs local analytical processing over Parquet and tens of millions of rows. DuckDB provides SQL, columnar execution, and direct Parquet access without requiring a persistent server.

A hosted relational/warehouse system could demonstrate deployment, but it would add infrastructure cost and complexity without automatically improving correctness or interview evidence.

## Verification Dataset

Create a tiny fixture containing:

- valid trips;
- invalid timestamp ordering;
- invalid distance;
- negative fare;
- missing location;
- suspicious speed;
- exact duplicate;
- enough valid rows to produce known mart aggregates.

## Required Automated Checks

1. raw fixture count is known;
2. clean + rejected behavior reconciles with the transformation contract;
3. every rejected row has a reason;
4. known valid rows survive cleaning;
5. known invalid rows are rejected;
6. mart aggregates match hand-computed expectations;
7. two identical runs produce equivalent counts and aggregates;
8. an intentional transformation bug causes a test failure.

## Performance Evidence

Correctness tests should use the tiny fixture. Performance measurement should use a larger representative run and record:

- row count;
- elapsed pipeline runtime;
- database/output size;
- peak memory if practical;
- raw query timing versus mart query timing for the same analytical question.

## ML Evaluation Contract

Before model metrics are promoted as strong resume evidence:

1. define when the prediction is made;
2. ensure all features would exist at that prediction time;
3. lock a deterministic train/test split or temporal holdout;
4. compare against a simple baseline;
5. report MAE/RMSE/R² consistently;
6. run an explicit leakage audit;
7. inspect errors by meaningful segments.

## Success Criteria

- a fresh environment can install dependencies and run the fixture pipeline;
- CI runs the fixture tests without downloading the full dataset;
- same-input reruns are proven equivalent;
- performance numbers come from a documented environment/run;
- model evaluation can be reproduced by one command or script.
