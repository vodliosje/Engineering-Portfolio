# RFC Summary — LIMS Digital Laboratory Workflow and Data Architecture

## Context

The laboratory originally relied on paper logs and Google Sheets for field and laboratory data entry. These methods required manual validation and created opportunities for typographical errors, incorrect row placement, inconsistent data entry, and duplicated records.

The laboratory already had established scientific procedures, naming conventions, and sample-identification rules. The software therefore needed to improve data entry without requiring researchers to replace the underlying laboratory workflow.

## Decision

Design a custom digital workflow with separate field and laboratory interfaces, backed by a nested Firebase/Firestore data model that follows the laboratory's existing site, visit, sample, and experiment relationships.

## Alternatives Considered

### Continue using paper logs and Google Sheets

Advantages:

- already familiar to researchers;
- little additional software maintenance.

Disadvantages:

- manual validation;
- inconsistent entry;
- additional transcription work;
- limited enforcement of workflow rules.

### Build a relational SQL-centered system

Advantages:

- explicit schemas and relationships;
- strong support for structured analytical queries.

Disadvantages:

- additional schema and infrastructure complexity for the application;

### Custom application with Firebase-backed nested records

Advantages:

- matches the existing hierarchical workflow;
- straightforward application integration;
- supports validation before writes;
- allows field and laboratory workflows to share a central data source.

Disadvantages:

- application becomes dependent on Firebase;
- nested records require careful documentation;
- confidential research data limits public reproducibility.

## Rationale

The custom application approach was selected because it allowed the laboratory to preserve its existing scientific procedures and identification conventions while improving consistency through standardized forms and built-in validation.

Separate field and laboratory workflows reduced the number of irrelevant inputs shown during each stage of the research process.

## Trade-offs

The system improves data consistency but requires continued software maintenance and workflow confirmation as laboratory procedures evolve.

Firebase reduces infrastructure overhead but creates a central external dependency and requires careful management of credentials and confidential data.

## Evidence

- desktop application;
- Android field application;
- validation logic;
- sanitized workflow screenshots;
- conceptual data model;
- synthetic validation examples;
- CSV export workflow.
