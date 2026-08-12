# Project Summary — Project 2: NYC Taxi Analytics Warehouse & Operations Dashboard

## 1. Problem

Project 2 analyzes NYC Yellow Taxi trip data to understand taxi demand, revenue, fare behavior, trip duration, tipping behavior, zone activity, and operational anomalies.

The dataset comes from the NYC Taxi & Limousine Commission trip record data. Each record represents a taxi trip and includes pickup/dropoff timestamps, pickup/dropoff location IDs, passenger count, trip distance, fare amount, tip amount, total amount, payment type, and related trip attributes.

The main analysis questions are:

1. When is taxi demand highest by hour, day, and month?
2. Which pickup and dropoff zones generate the most trips and revenue?
3. How do distance, duration, fare, and tip relate?
4. Which trips are invalid, suspicious, or operationally unusual?
5. How does tipping vary by payment type, time, and zone?
6. Can a beginner regression model predict fare or trip duration?

The project needs a pipeline instead of only notebook analysis because the dataset can reach tens of millions of records. A notebook-only approach would be harder to reproduce, slower to scale, and less reliable for dashboarding. The project separates raw data, cleaned data, rejected records, marts, analysis notebooks, SQL checks, and dashboard outputs.

## 2. Users / Context

The imagined users are a city mobility team, transportation operations analysts, and data stakeholders who need to understand taxi demand, revenue, trip behavior, and data quality.

The dashboard and analysis are designed for users who want to answer questions like:

- When are taxis most demanded?
- Which zones are operational hotspots?
- Which fare or trip patterns look abnormal?
- Which payment types are associated with higher tips?
- How reliable is the cleaned dataset?
- Where does a simple fare prediction model work or fail?

This project is positioned as an operations analytics and big-data workflow project, not just a visualization notebook.

## 3. What I Built

I built a scalable NYC Taxi analytics workflow with three development layers.

First, I prototyped the logic with a one-month Pandas version to understand the schema, cleaning rules, derived features, and analysis questions.

Second, I rebuilt the same logic using DuckDB and SQL to create a warehouse-style workflow.

Third, I scaled the DuckDB version to a larger multi-month or full-year dataset.

Main deliverables include:

- Raw NYC taxi Parquet data layer
- DuckDB database: `taxi.duckdb`
- Raw trip view/table
- Cleaned taxi trip table
- Rejected trip table
- Aggregated mart tables
- SQL validation checks
- EDA notebooks
- Performance notes/report
- Streamlit dashboard
- Beginner regression model for fare prediction or duration prediction

The pipeline creates analytical marts such as:

- Hourly demand mart
- Daily revenue mart
- Monthly trend mart
- Pickup zone demand mart
- Dropoff zone demand mart
- Payment/tip behavior mart
- Anomaly mart
- Modeling dataset for regression

## 4. My Ownership

I owned the full project design and implementation.

My work included:

- Defining the operations analytics problem
- Setting up the project structure
- Downloading and organizing public NYC taxi data
- Creating the Pandas prototype
- Designing cleaning rules
- Creating derived trip features
- Separating clean and rejected trips
- Building DuckDB SQL views/tables
- Creating aggregated mart tables
- Writing SQL validation checks
- Building EDA notebooks
- Creating dashboard-ready outputs
- Building the Streamlit dashboard
- Running a beginner regression model
- Evaluating model performance
- Comparing baseline vs Linear Regression
- Interpreting model error patterns

The dataset is a public NYC Taxi & Limousine Commission dataset. The analysis, cleaning logic, SQL layer, marts, dashboard, and model evaluation were designed and implemented by me.

## 5. Architecture / Data Flow

The project data flow is:

```text
NYC Taxi Parquet files
↓
Raw data folder
↓
Pandas prototype / DuckDB raw view
↓
Schema and data quality profiling
↓
Rejected trip table
↓
Clean trip table
↓
Derived features
↓
Aggregated marts
↓
SQL analysis / EDA notebooks
↓
Dashboard
↓
Regression modeling bridge
```

More specifically:

```text
data/raw/yellow/*.parquet
↓
raw_yellow_trips
↓
clean_yellow_trips + rejected_trips
↓
mart_hourly_demand
mart_daily_revenue
mart_zone_pickup_demand
mart_zone_dropoff_demand
mart_payment_tip_behavior
mart_anomalies
↓
Streamlit dashboard + ML notebook
```

The raw data remains unchanged. Cleaning and transformation create separate processed tables. Dashboarding uses aggregated marts instead of repeatedly scanning raw trip records.

## 6. Key Technical Decisions

I used DuckDB because Project 2 is designed to practice larger-scale analytics. DuckDB can query Parquet files directly and lets me build a local analytics warehouse without loading all raw rows into Pandas.

I used Pandas first in Project 2.1A because it helped me understand the schema, cleaning rules, data quality issues, and feature logic on a smaller one-month dataset.

I used DuckDB in Project 2.1B and 2.2 because it is better suited for scaling to many months of taxi data. DuckDB handles SQL transformations, aggregation, and mart creation more efficiently than doing all heavy work in Pandas.

I used Parquet because the NYC taxi data is already distributed in Parquet format and Parquet is efficient for columnar analytics.

I separated Python/Pandas analysis from DuckDB/SQL warehouse logic because they serve different purposes:

- Pandas: prototyping, inspection, EDA, plotting, modeling samples
- DuckDB/SQL: large-scale cleaning, validation, aggregation, marts
- Streamlit: business-facing dashboard

I created intermediate and processed layers because the project needs reproducibility and auditability:

- Raw layer: original data
- Clean layer: valid trips
- Rejected layer: invalid records with reasons
- Mart layer: dashboard-ready aggregates
- Modeling layer: selected features and target for regression

## 7. Validation / Reliability

The project validates data quality using checks such as:

- Missing pickup datetime
- Missing dropoff datetime
- Dropoff before or equal to pickup
- Trip duration less than or equal to zero
- Extremely high trip duration
- Trip distance less than or equal to zero
- Extremely high trip distance
- Fare amount less than zero
- Total amount less than zero
- Tip amount less than zero
- Passenger count outside reasonable range
- Missing pickup location
- Missing dropoff location
- Impossible or suspicious speed
- Exact duplicate rows
- Trip-like duplicate rows

Invalid records are not silently deleted. They are separated into a rejected trip table with rejection reasons.

Important validation logic includes:

```text
Raw trips remain unchanged.
Clean trips contain only records that pass core validity rules.
Rejected trips preserve invalid records for audit.
Mart tables are created from clean trips, not raw trips.
Dashboard pages use marts or cleaned data, not rejected records for normal business metrics.
```

I also checked exact duplicate rows. In one large-scale run, the exact duplicate check returned only one duplicate copy across nearly 49 million records, meaning full-row duplication was not a major issue. Trip-like duplicates still require separate checking because two records may describe the same trip even if not every column matches exactly.

Idempotency expectation:

```text
If the pipeline is run twice with the same input files, the output tables should be replaced or regenerated consistently, producing the same row counts, clean/rejected counts, and mart summaries.
```

Current reliability is based on SQL validation checks and notebook checks. A full automated test suite is not yet implemented.

## 8. Results / Scale

The large-scale version works at approximately tens of millions of taxi trip records. In the current Project 2.2 work, the raw dataset appears to be around 48.7 million records based on the large duplicate-check output.

Core outputs include:

- Raw trip view/table
- Clean trip table
- Rejected trip table
- Aggregated mart tables
- Dashboard pages
- Regression model evaluation

The beginner ML bridge compared a baseline mean model against Linear Regression.

One stronger Linear Regression result showed:

```text
Baseline MAE: $11.941
Baseline RMSE: $18.176
Baseline R²: approximately 0.000

Linear Regression MAE: $3.123
Linear Regression RMSE: $6.002
Linear Regression R²: 0.891
```

This means the Linear Regression model significantly outperformed the baseline and explained a large share of fare variation.

The strongest model result showed that trip distance was the most important feature, with the model performing well on normal short and medium trips but struggling more on long-distance or high-fare trips.

A weaker model experiment showed:

```text
Baseline MAE: $10.631
Baseline RMSE: $16.737
Baseline R²: approximately 0.000

Linear Regression MAE: $9.369
Linear Regression RMSE: $15.767
Linear Regression R²: 0.112
```

This weaker result suggested that the feature preparation or model setup did not capture the fare-distance relationship well, especially because the trip distance coefficient was near zero.

The project therefore includes not only model training, but also model comparison and error analysis.

## 9. Limitations

This project is not production-ready yet.

Current limitations include:

- Some paths may still depend on local project structure.
- Dataset license/source documentation should be clearly linked in the README.
- Runtime and memory usage have not been fully benchmarked yet.
- Performance reporting should be expanded with query timing.
- Current validation is mainly SQL/notebook-based, not a full automated test suite.
- Some categorical variables such as location IDs and payment type may need better encoding for ML.
- Linear Regression may not handle airport trips, flat-rate trips, toll-heavy trips, or extreme outliers well.
- Weather, traffic, holidays, and event data are not included.
- Taxi zone shapefile/map visualization may be added later.
- The dashboard should read from marts for scale, but some early prototype views may still query larger tables.
- The ML model is a beginner regression bridge, not a production fare prediction system.

The project is best described as an analytics warehouse and operations dashboard project with a beginner ML bridge.

## 10. Evidence

Strongest evidence for this project includes:

- `src/` pipeline scripts
- DuckDB build script
- SQL files for raw views, cleaning, marts, and validation
- Data quality report
- Performance report or performance notes
- EDA notebooks
- Regression notebook
- Streamlit dashboard
- Mart table outputs
- README with run instructions
- Screenshots of dashboard pages
- Model evaluation tables and plots
- Error analysis charts
- Data flow diagram

Most important files to show:

```text
src/build_warehouse.py
src/validate.py
sql/01_create_views.sql
sql/02_clean_trips.sql
sql/03_create_marts.sql
sql/04_analysis_queries.sql
notebooks/02_big_scale_eda.ipynb
notebooks/03_regression_bridge.ipynb
dashboard/app.py
reports/big_data_quality_report.md
reports/performance_report.md
README.md
```

Strongest resume framing:

Built a DuckDB-based NYC Taxi analytics warehouse and Streamlit operations dashboard to analyze demand, revenue, fares, tipping behavior, pickup/dropoff zones, anomalies, and beginner fare prediction across tens of millions of taxi trip records. Designed raw, clean, rejected, and mart layers; created SQL validation checks; built aggregated dashboard-ready tables; and evaluated a baseline vs Linear Regression model using MAE, RMSE, R², and error analysis.

Possible resume bullets:

- Built a DuckDB analytics warehouse for NYC Yellow Taxi trip data, processing approximately 48M+ trip records from Parquet files into raw, clean, rejected, and dashboard-ready mart layers.
- Designed SQL validation checks for invalid durations, invalid distances, negative fares, missing locations, suspicious speeds, and duplicate records.
- Created aggregated mart tables for hourly demand, daily/monthly revenue, pickup/dropoff zone activity, payment-tip behavior, and anomaly analysis.
- Developed a Streamlit operations dashboard showing taxi demand patterns, revenue trends, zone performance, tip behavior, and data quality metrics.
- Built a beginner fare prediction model using trip distance, duration, time, passenger count, payment type, and location features; improved MAE from approximately $11.94 baseline to $3.12 with Linear Regression in the strongest experiment.
- Compared Pandas prototyping against DuckDB-based warehouse design to demonstrate scalable analytics practices and performance-aware data processing.
