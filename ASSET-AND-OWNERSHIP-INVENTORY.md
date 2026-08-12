# Asset and Ownership Inventory

| Project    | Artifact / Claim                              | Ownership       | Public? | Sensitive / Risk / Note                        | Status  | Next Action                                        |
| ---------- | --------------------------------------------- | --------------- | ------- | ---------------------------------------------- | ------- | -------------------------------------------------- |
| LIMS       | Core application source code                  | Me              | No      | Confidential laboratory data                   | VERIFY  | Separate public-safe code/evidence                 |
| LIMS       | Firestore data persistence & record lifecycle | Me              | No      | Using Firebase code library                    | VERIFY  | Confirm ownership and link safe code               |
| LIMS       | Data validation & business rules              | Me              | No      | Implemented base on existing entry errors      | VERIFY  | Extract representative validation cases            |
| LIMS       | Login & permission-based access               | Me              | No      | Real credentials must remain private           | VERIFY  | Document behavior without exposing users/passwords |
| LIMS       | Scientific data-entry workflow & UI           | Me              | Yes     | Screenshots may contain real research data     | VERIFY  | Use synthetic or sanitized evidence                |
| LIMS       | Site / visit / sample data model              | Team            | No      | Implemented on existing lab's indentify system | VERIFY  | Document structure with synthetic examples         |
| LIMS       | Data viewing / export workflow                | Me              | No      | Output may contain confidential data           | VERIFY  | Inspect viewer/export code separately              |
| LIMS       | 95% validation improvement claim              | TBD             | Yes     | Metric methodology not yet verified            | VERIFY  | Reconstruct baseline, denominator, and formula     |
| LIMS       | Service-account / credential files            | Project/private | NO      | Secrets                                        | PRIVATE | Keep out of public repos                           |
| Retail     | ETL pipeline                                  | Mine            | Yes     | Low risk                                       | READY   | Add repo/code link                                 |
| Retail     | Dataset                                       | Public source   | Maybe   | Check license / redistribution rights          | VERIFY  | Confirm dataset source and license                 |
| Retail     | 541K+ record claim                            | Mine            | Yes     | Exact count should be reproducible             | VERIFY  | Confirm row count                                  |
| Retail     | Benchmark                                     | Mine            | Yes     | No current baseline                            | MISSING | Run benchmark later                                |
| Retail     | Idempotency test                              | Mine            | Yes     | Not yet documented/tested                      | MISSING | Create test                                        |
| PlantSight | Source code                                   | Team            | Maybe   | Need clear ownership breakdown                 | VERIFY  | Identify my exact contribution                     |
| PlantSight | OpenWeather integration                       | Mine            | Yes     | Low risk                                       | READY   | Add code link                                      |
| PlantSight | Perenual integration                          | Mine            | Yes     | Low risk                                       | READY   | Add code link                                      |
| PlantSight | Screenshots                                   | Team/project    | Maybe   | Check names, emails, keys, private data        | FIX     | Review before publishing                           |
| PlantSight | API keys / `.env`                             | Personal        | NO      | Secret credentials                             | PRIVATE | Never publish                                      |
| PlantSight | Architecture diagram                          | Mine / team     | Yes     | Label team work accurately                     | VERIFY  | Document ownership                                 |
