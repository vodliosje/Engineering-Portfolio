# Freshwater Sponge Laboratory Information Management System (LIMS)

## 1. Problem

The environmental research workflow relied heavily on paper logs and spreadsheets for field and laboratory data entry. That created recurring risks: transcription mistakes, inconsistent formatting, records entered under the wrong category or sequence, duplicate entries, and repeated manual validation.

I developed a custom information-management application to standardize those workflows and validate records before database writes while preserving the laboratory's existing sample-identification conventions.

## 2. Users and Context

The system supports a research group of approximately 10–15 faculty, staff, near-peer mentors, and undergraduate researchers.

Two interfaces serve different operating contexts:

- a Python/PyQt desktop application for laboratory entry, review, and export;
- an Android application for field collection away from laboratory computers.

Paper records remain a backup and research reference rather than being fully eliminated.

## 3. What I Built

I designed and implemented the software around the laboratory's existing forms and research procedures.

The system includes:

- Python desktop application, migrated from Tkinter to PyQt as the interface grew;
- Android field-data interface;
- Firebase-backed NoSQL persistence;
- workflow-specific data-entry screens;
- validation before writes;
- duplicate-record checks;
- record viewing and selection;
- one-click CSV export for downstream analysis and reporting.

## 4. My Ownership

Laboratory supervisors defined research procedures and application requirements. I owned the software architecture and primary implementation, including:

- desktop framework selection and migration;
- Firebase integration and application-side data architecture;
- desktop and Android implementation;
- field and laboratory workflows;
- validation logic;
- database reads/writes;
- CSV export functionality.

The site's naming and identification conventions came from the laboratory. My engineering responsibility was to encode those conventions into software without forcing researchers to adopt a different scientific workflow.

## 5. Data Model and Scale

The conceptual hierarchy is:

```text
Site
└── Visit
    └── Physical Sample
        ├── Basic / field information
        ├── Water records
        ├── Sediment records
        ├── Identity records
        └── DNA records
```

The system contains data associated with more than **1,200 physical samples**. A physical sample can produce multiple experiment or identification records, so those samples correspond to more than **4,000 associated laboratory records** across water, sediment, identity, and DNA workflows.

This distinction matters: a sample is a physical/research entity, while a record is one piece of data associated with that sample.

## 6. Data Flow

### Field workflow

```text
User input
  ↓
GPS / naming / logic validation
  ↓
Record creation using existing lab conventions
  ↓
Firebase
```

### Laboratory workflow

```text
Existing sample
  ↓
Select laboratory workflow
  ├── Identity
  ├── Water
  ├── Sediment
  └── DNA
  ↓
Validation
  ↓
Firebase
```

### Export workflow

```text
Firebase
  ↓
User selects export scope
  ↓
Filtering / transformation
  ↓
CSV output
```

## 7. Key Engineering Decisions

### Preserve the laboratory's workflow instead of redesigning the science

The highest-risk product decision was not database selection; it was fitting software around an established research process. A technically cleaner interface that changed identifiers or sequence rules would have increased training cost and adoption risk.

The application therefore preserves existing conventions while adding validation and more consistent data entry.

### Separate field and laboratory interfaces

Field collection and laboratory analysis have different interaction constraints. The Android interface supports field entry, while the desktop application supports denser laboratory workflows and review/export tasks.

### Use managed Firebase persistence

Firebase reduced infrastructure overhead for a small research project and supported the required application integrations. The trade-off is that application-side validation, authorization, failure behavior, and data-access rules must be tested deliberately rather than assumed.

### Use a hierarchical NoSQL representation

The research data naturally groups around site → visit → sample → experiment records. A nested representation keeps related experiment outputs associated with the physical sample while matching the laboratory's existing conventions.

## 8. Validation and Correctness

Current validation includes representative rules such as:

- required-field checks;
- numeric formatting/precision requirements;
- expected scientific ranges;
- geographic/GPS constraints;
- sample/site naming conventions;
- duplicate-record checks.

When an existing logical record is detected, the application prevents accidental duplicate creation and directs the researcher to the appropriate existing or subsequent workflow.

The next evidence milestone is to convert representative rules into automated tests using synthetic fixtures so correctness can be demonstrated publicly without exposing research data.

## 9. Privacy and Security Boundary

The production source and research data are not published because they may contain confidential laboratory information, credentials, identifiers, locations, or experiment data.

Public evidence must therefore use:

- synthetic sample records;
- sanitized screenshots;
- conceptual architecture;
- non-sensitive workflow examples;
- test accounts or mocks where appropriate.

No real credentials, service-account files, private database contents, or sensitive laboratory exports should be included in this portfolio.

## 10. Current Limitations

- automated test coverage is still being expanded;
- dependency-failure behavior is not yet publicly demonstrated;
- authorization/security-rule behavior needs reproducible tests;
- the project has primarily been maintained by one developer, increasing review and maintenance risk;
- source-level public review is constrained by research confidentiality.

## 11. Strongest Evidence to Build Next

1. Synthetic fixture covering site, visit, sample, and multiple experiment records.
2. Automated required-field, range, GPS, naming, and duplicate tests.
3. Legal/illegal workflow-transition tests.
4. Authorization test with permitted and denied roles/accounts.
5. Firebase unavailable/failed-write test.
6. Synthetic CSV export demonstration.
7. Supervisor factual confirmation of role, user scope, and conservative scale claims if available.

See [evidence-links.md](./evidence-links.md) for the current verification index.
