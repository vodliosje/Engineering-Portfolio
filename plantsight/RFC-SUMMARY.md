# RFC Summary — PlantSight Backend and Campus Visualization Architecture

## Context

PlantSight was developed during a short hackathon under the theme “Nature & Touching Grass.”

The team needed to implement user accounts, plant ratings, residence-hall scores, uploaded content, and campus visualization within a limited development window.

## Decision

Use Firebase as the application's central data/backend service and Google Maps as a separate visualization layer for participating residence halls.

The rating and scoring workflow remains centered on Firebase rather than depending directly on the map.

## Alternatives Considered

### Build a custom backend and database

Advantages:

- greater control over APIs and data behavior;
- easier to customize server-side logic.

Disadvantages:

- substantially more infrastructure and deployment work;
- higher implementation cost during a hackathon.

### Store state only in the browser

Advantages:

- very fast prototype development.

Disadvantages:

- no shared state between users;
- no persistent competition scores;
- unsuitable for user-generated content.

### Firebase + Google Maps

Advantages:

- rapid integration;
- centralized application data;
- supports authentication, scores, plants, and uploaded content;
- Google Maps provides a familiar campus visualization.

Disadvantages:

- Firebase becomes a critical dependency;
- limited fallback behavior;
- security and validation require additional work beyond the hackathon prototype.

## Rationale

The architecture prioritized delivering a functional multi-user prototype within the hackathon time constraint.

Firebase reduced the need to design and deploy a separate backend, while Google Maps provided geographic context without becoming part of the application's core score-calculation logic.

## Trade-offs

This choice accelerated development but left several production concerns unresolved, including stronger validation, authorization, security rules, and dependency-failure handling.

The team also prioritized core functionality over UI/UX polish.

## Evidence

- Firebase integration;
- authentication implementation;
- plant-rating workflow;
- residence-hall score calculation;
- Google Maps integration;
- image-upload flow;
- source repository;
- demo screenshots.
