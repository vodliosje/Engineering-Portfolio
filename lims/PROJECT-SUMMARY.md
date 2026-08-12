# Project Summary

## 1. Problem

Before the application was introduced, laboratory data was primarily recorded through paper logs and Google Sheets. This workflow created several risks, including lost records, transcription errors, inconsistent formatting, and manual validation overhead.

Common errors included typographical mistakes, entering values in the wrong row, and recording data under the wrong category or sequence.

To reduce these problems, I developed a custom application that provides a standardized interface across researchers and performs validation before records are written to the database.

## 2. Users / Context

The system was developed for a research laboratory of approximately 10–15 faculty, staff, near-peer mentors, and undergraduate researchers.

The desktop application is used during laboratory experiments to record and review results. A separate Android application supports field data collection when researchers are working away from laboratory computers.

Paper logs are still maintained as a backup and reference source.

## 3. What I Built

Based on existing paper forms and requirements provided by the research team, I designed and developed a Python desktop application for laboratory data entry and management.

The first version was built with Tkinter. As the interface and workflow became more complex, I migrated the desktop application to PyQt to improve maintainability and support the required user interface more effectively.

I also developed an Android application for field data collection.

Laboratory records are stored in Firebase using a nested NoSQL/JSON-style data structure. The system includes validation before database writes and supports one-click CSV exports for downstream analysis, presentations, and preparation of research materials.

## 4. My Ownership

The laboratory supervisors defined the application requirements and existing research procedures, while I was responsible for designing and implementing the software architecture.

My responsibilities included:

- selecting and implementing the desktop framework;
- designing the Firebase-backed data architecture;
- developing the desktop and Android applications;
- implementing field and laboratory workflows;
- implementing data-entry validation;
- implementing database read/write logic;
- implementing CSV export functionality.

The data model was designed around the laboratory's existing naming and identification conventions so that the software could integrate with established research procedures without requiring researchers to adopt a completely new system.

Validation rules were derived from existing laboratory procedures and observed data-entry problems. Examples include numeric precision requirements, valid scientific ranges, GPS constraints, naming conventions, and duplicate-record checks.

## 5. Architecture / Data Flow

### Field Pipeline

```text
User Input
    ↓
GPS / Naming / Logic Validation
    ↓
Record Creation Using Existing Lab Format
    ↓
Firebase
```

### Laboratory Pipeline

```text
Firebase
    ↓
Select Existing Record
    ↓
+-----------------------------+
| Identity Results            |
| Water Experiment Results    |
| Sediment Results            |
| DNA Results                 |
+-----------------------------+
    ↓
Validation
    ↓
Firebase
```

### Export Pipeline

```text
Firebase
    ↓
User Selects Export Options
    ↓
Filtering / Transformation
    ↓
CSV File
```

### Workflow Logic

```text
Field Record
    ↓
New Sample
    ↓
Sponge Collected?
    ├── Yes → Identity Test
    │          └── Unable to Identify → DNA Test
    │
    ├── Water Collected? → Water Experiment
    │
    └── Sediment Collected? → Sediment Experiment
```

### Conceptual Data Model

The following diagram represents the logical relationship between sites, visits, samples, and experiment results. It is intended to explain the application's data model conceptually and does not necessarily reproduce the exact Firebase document/path structure.

```text
Site
└── Visit
    └── Sample
        ├── Basic Information
        ├── Water
        │   └── Experiment Results
        ├── Sediment
        │   └── Experiment Results
        ├── Identity
        │   └── Identification Results
        └── DNA
            └── Experiment Results
```

This hierarchy reflects how related environmental and laboratory records are grouped around each physical sample while preserving the laboratory's existing site, visit, and sample identification conventions.

## 6. Key Technical Decisions

### Firebase

Firebase was selected because it provided a managed database service with straightforward Python integration and a usage-based pricing model suitable for a research project with limited infrastructure requirements.

### Desktop and Android Applications

Separate desktop and Android interfaces were used because laboratory and field workflows have different constraints.

The desktop application supports detailed laboratory data entry and review, while the Android application supports data collection during field work where access to a laboratory computer may not be practical.

The application is not exposed as a public web application, which also helps limit unnecessary exposure of confidential research data.

### NoSQL Data Model

A nested NoSQL structure was selected because the laboratory data naturally follows a hierarchy of:

```text
site → visit → sample → experiment results
```

This structure allowed related water, sediment, identity, and DNA records to remain associated with the same sample.

### Field and Laboratory Modes

Field and laboratory workflows were separated to reduce accidental data entry into the wrong sections and to present researchers only with the inputs relevant to their current task.

### Most Difficult Design Decision

One of the most difficult decisions was designing an interface that matched the laboratory's existing data-entry procedures closely enough to reduce training and transition costs while still improving validation and consistency.

## 7. Validation / Reliability

The application performs validation before new data is written to Firebase.

Representative validation rules include:

- formatting selected numeric measurements to two decimal places;
- checking scientific values against expected ranges for specific experiments;
- validating GPS coordinates against expected geographic constraints;
- checking sample and site naming against the laboratory's existing identification system;
- preventing duplicate record creation;
- checking required fields before submission.

When an existing record is detected, the application prevents accidental duplicate creation and requires the researcher to use the appropriate existing record or subsequent workflow.

## 8. Results / Scale

The laboratory system contains data associated with more than **1,200 physical samples** collected across multiple sites and visits.

Because each sample can contain multiple experiment and identification records, the application has processed more than **4,000 nested laboratory records** across water, sediment, DNA, and identity workflows.

The application reduced dependence on manual spreadsheet entry by providing standardized digital forms and automated validation.

Paper records continue to be used as a backup and research reference rather than being completely replaced.

The application also supports one-click CSV exports for downstream analysis and reporting.

The exact record counts are available internally but are not publicly disclosed because the underlying research data is confidential.

## 9. Limitations

Current limitations include:

- significant development time is required to confirm and encode the workflow for each type of laboratory experiment;
- the software was primarily developed and maintained by one developer, creating a maintenance and review bottleneck;
- laboratory data and most raw screenshots cannot be published because the research data is confidential;
- paper records are still maintained as a parallel backup system;
- broader automated testing and reproducibility evidence are still being developed.

## 10. Evidence

Public or sanitized evidence planned for this project includes:

- sanitized desktop application screenshots;
- sanitized Android application screenshots;
- architecture diagram;
- synthetic validation examples;
- synthetic sample records;
- representative workflow diagrams;
- CSV export example using non-sensitive data;
- validation test cases using synthetic fixtures.
