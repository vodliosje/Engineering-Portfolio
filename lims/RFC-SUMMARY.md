# Engineering RFC — LIMS Reliability and Public Evidence Slice

## Decision

Keep the existing application architecture and build a **public-safe verification slice** around it instead of rewriting the LIMS or publishing production research data.

## Goals

- demonstrate software-enforced workflow and validation rules;
- prove representative correctness with automated tests;
- demonstrate access control and dependency-failure behavior;
- preserve confidentiality of production data and credentials;
- make the strongest SWE claims independently understandable.

## Non-Goals

- migrating the application to a new framework;
- replacing Firebase solely for portfolio value;
- publishing production data or private source;
- reconstructing the retired 95% validation-improvement claim;
- adding unrelated cloud/Kubernetes infrastructure.

## Current Architecture

```text
Android field client ─┐
                      ├── Firebase-backed research data
PyQt desktop client ──┘
          │
          └── CSV export for downstream analysis
```

Application-side workflow and validation logic sits between user input and database writes.

## Alternatives Considered

### A. Rewrite as a public web application

**Advantage:** easy to demo and host.

**Rejected for now:** high effort, creates a second codebase, and does not prove the original production system is reliable.

### B. Publish the production repository

**Advantage:** strongest source-code transparency.

**Rejected:** confidentiality and credential/data risk are more important than portfolio convenience.

### C. Build synthetic public evidence around the existing system — chosen

**Advantages:**

- directly validates the architecture already used;
- small, reviewable scope;
- safe for research privacy;
- produces tests that improve the real engineering floor.

## Verification Slice

Use a synthetic data fixture with a small number of sites, visits, physical samples, and experiment records.

Required tests:

1. required field missing → reject;
2. value outside an allowed scientific range → reject;
3. invalid GPS/naming convention → reject;
4. duplicate logical record → reject/no duplicate write;
5. legal workflow transition → allow;
6. illegal transition → reject;
7. unauthorized user/action → reject;
8. Firebase/network write failure → no false success or partial state.

## Success Criteria

The slice is considered complete when:

- tests run without production data;
- at least one intentional bug causes the suite to fail;
- a clean test environment can reproduce the results;
- no secrets or confidential research records are required;
- a short demo can show one valid path and one rejected path.

## Risks

- synthetic fixtures can diverge from real workflow rules;
- mocks can hide actual Firebase behavior;
- single-developer ownership can bias test selection.

Mitigation: use production-derived rules without production data, include at least one integration-level check where safe, and seek supervisor/technical review of factual claims.
