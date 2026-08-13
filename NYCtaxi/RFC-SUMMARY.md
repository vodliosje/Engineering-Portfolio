# RFC Summary — NYC Taxi DuckDB Warehouse and Layered Parquet Architecture

## Context

The project processes more than 48 million NYC taxi trip records from the 2025 dataset.

The raw data is distributed across monthly files and contains records that require cleaning, filtering, transformation, and aggregation before the data can support analytical queries and dashboards.

Running repeated analysis directly on raw CSV files would create unnecessary processing overhead and make downstream analysis harder to maintain.

## Decision

Use a layered analytical architecture built around DuckDB and Parquet:

```text
Monthly Raw Files
        ↓
Raw / Ingestion Layer
        ↓
Cleaning and Validation
        ↓
Staging Layer
        ↓
Parquet
        ↓
DuckDB Analytical Warehouse
        ↓
Aggregated Data Marts
        ↓
Streamlit Dashboard / Analysis
```

The pipeline separates raw ingestion, cleaned data, analytical marts, and presentation workloads rather than querying the original files for every analysis.

## Alternatives Considered

### Query raw CSV files directly

Advantages:

- minimal preprocessing;
- simple initial workflow.

Disadvantages:

- repeated CSV parsing;
- slower analytical queries;
- transformations must be repeated;
- difficult to maintain consistent cleaned datasets.

### Load everything into a traditional database server

Advantages:

- mature relational database capabilities;
- strong support for concurrent applications.

Disadvantages:

- requires additional database infrastructure;
- unnecessary operational overhead for a local analytical project;
- more setup and maintenance than required for the workload.

### DuckDB with Parquet-backed analytical layers

Advantages:

- efficient analytical processing on local hardware;
- direct support for Parquet;
- SQL-based transformations and analysis;
- avoids maintaining a separate database server;
- allows large datasets to be processed using columnar storage;
- supports reusable aggregated marts for dashboards.

Disadvantages:

- primarily designed for analytical rather than transactional workloads;
- local hardware remains a resource constraint;
- pipeline performance must still be benchmarked and monitored.

## Rationale

DuckDB was selected because the project is primarily an analytical workload rather than a transactional application.

Parquet provides a compact columnar format that reduces repeated parsing of the original monthly files and supports efficient analytical queries.

Separating the system into raw, staging, and mart layers also makes the transformation process easier to inspect and allows dashboard queries to use pre-aggregated datasets rather than repeatedly scanning the full dataset.

## Data Layers

### Raw

Original 2025 NYC taxi records collected from monthly source files.

### Staging

Cleaned and standardized trip records after removing impossible or invalid observations and preparing fields for analysis.

### Analytical Marts

Aggregated tables designed for specific analytical workloads, including areas such as:

- hourly demand;
- daily activity;
- fare and revenue analysis;
- tipping behavior;
- pickup patterns;
- drop-off patterns;
- trip-duration analysis.

## Trade-offs

The layered architecture increases the amount of project structure and creates additional intermediate datasets.

However, this complexity is justified by improved query reuse, easier debugging, clearer data lineage, and reduced dashboard computation.

The project currently requires stronger formal benchmarking for runtime and peak memory usage.

## Evidence

- 2025 NYC taxi source data;
- raw/staging/mart schema;
- DuckDB SQL transformations;
- Parquet outputs;
- row-count reconciliation;
- cleaning and filtering logic;
- analytical data marts;
- Streamlit dashboard;
- runtime benchmark to be added.
