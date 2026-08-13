# PlantSight — Sustainability Hackathon Web Application

## 1. Problem

PlantSight was developed by a three-person team during GeauxHack at Louisiana State University under the theme **“Nature & Touching Grass.”**

The project explored a lightweight campus competition designed to encourage students to notice and interact with green spaces. Residence halls could accumulate scores based on plant ratings and user participation.

## 2. Prototype Scope

The completed hackathon prototype included:

- user accounts and login;
- residence-hall visualization using Google Maps;
- plant information for more than 100 species;
- 1–5 plant ratings;
- residence-hall score updates;
- image uploads and associated information;
- social-style user-generated content.

OpenWeather and Perenual were considered during design but were **not implemented in the completed hackathon prototype**.

## 3. Technology

- JavaScript web application;
- Firebase for authentication/data/storage-related application functionality;
- Google Maps for campus visualization.

Source repository: https://github.com/vodliosje/VieGanG-geaux

## 4. My Ownership

Because this was a team project, ownership is separated explicitly.

### I directly implemented

- Firebase integration across the application;
- user authentication/login functionality;
- plant-rating workflow;
- residence-hall score calculation and updates;
- logic for displaying plants for rating;
- portions of Google Maps integration.

### Shared design/implementation

- overall concept;
- campus competition design;
- portions of Google Maps functionality;
- general UI decisions.

### Teammate contributions

Teammates contributed to areas including:

- portions of Google Maps implementation;
- image upload;
- social-style interactions;
- UI implementation;
- presentation/demo work.

The competition concept was collaborative. My main implementation contribution to that concept was the rating/scoring and Firebase-backed workflow.

## 5. Architecture

```text
Browser application
      │
      ├── Authentication ── Firebase
      │
      ├── Rating / score logic ── Firebase data
      │
      ├── User-generated content ── Firebase data/storage
      │
      └── Campus visualization ── Google Maps
```

### Rating flow

```text
Authenticated user
  ↓
Select hall / plant
  ↓
Choose rating (1–5)
  ↓
Rating logic
  ↓
Update residence-hall score
  ↓
Firebase
  ↓
Updated score displayed
```

## 6. Key Engineering Decisions

### Firebase for hackathon velocity

A managed backend let the team implement authentication and shared application state without spending the short hackathon window building and deploying a custom server.

The trade-off is that Firebase becomes a central dependency and requires deliberate authorization, data-integrity, and failure-state handling.

### Google Maps as a presentation layer

The map communicates campus location and participating residence halls, but the core score data is logically separate from the map. This separation is useful for graceful degradation: a map failure should not corrupt rating data.

### Prioritize working vertical slices over UI polish

The hackathon constraint favored authentication, data integration, mapping, rating, scoring, and content upload over production-level styling, testing, or reliability engineering.

## 7. Reliability and Security Gaps

The original prototype was manually tested during hackathon development, but it does not yet have a strong automated reliability floor.

Important gaps include:

- incomplete automated rating/input validation;
- incomplete authorization/security-rule testing;
- Firebase as a central dependency with limited fallback behavior;
- limited handling when Google Maps fails;
- potential concurrent score-update/lost-update behavior that needs testing;
- public client/API configuration that should be reviewed for appropriate restrictions.

These are useful hardening targets because they turn a hackathon prototype into evidence of production-aware SWE judgment without rewriting the application.

## 8. Results and Limitations

The team completed and demonstrated a working prototype during the hackathon. The project did not produce a defensible long-term user-impact metric because it was not deployed to a sustained user population.

The project should therefore be positioned around:

- full-stack integration under time constraints;
- team collaboration;
- clearly separated individual ownership;
- authentication and shared state;
- future hardening of validation, security, concurrency, and failure behavior.

It should **not** be positioned as a production-scale application or as having implemented OpenWeather/Perenual during the hackathon.

## 9. Strongest Evidence to Build Next

1. Clean clone/install/run path.
2. Authentication logged-in/logged-out/error tests.
3. Rating boundary and score-calculation tests.
4. Concurrent two-user score-update test.
5. Firebase failed-write/unavailable test.
6. Google Maps unavailable/error-state test.
7. Firebase authorization/security-rule tests.
8. Teammate factual confirmation of individual ownership.
9. One small reviewed hardening change if feasible.

See [evidence-links.md](./evidence-links.md) for the verification index.
