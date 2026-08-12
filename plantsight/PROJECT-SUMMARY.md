# PlantSight — Project Summary

## 1. Problem

PlantSight was developed during GeauxHack at Louisiana State University under the theme **“Nature & Touching Grass.”**

The project focused on encouraging students and young adults who live and study around university campuses to pay more attention to green spaces and plant life around them.

The team designed a campus-based competition in which residence halls could be compared through plant ratings and accumulated scores. The goal was to turn interaction with campus green spaces into a lightweight social and competitive experience.

## 2. Users / Context

The target users were university students living or studying around campus.

The prototype allowed users to:

- create and access user accounts;
- view participating residence halls on a map;
- view plants associated with residence halls;
- rate plants from 1 to 5;
- update residence-hall scores based on submitted ratings;
- upload images and associated information;
- compare participating residence halls through accumulated scores.

The project was developed as a team hackathon prototype rather than a production application.

## 3. What We Built

PlantSight was built as a web application using JavaScript, Firebase, and Google Maps.

The application combined three main ideas:

1. **Campus visualization** — Google Maps was used to display participating residence halls.
2. **Plant rating and competition** — users could rate plants associated with a residence hall, contributing to that hall's score.
3. **User-generated content** — users could upload images and information associated with plants, residence halls, and user accounts.

Firebase served as the application's central backend for user information, plant information, uploaded content, and residence-hall scores.

OpenWeather and Perenual were considered as planned integrations during the project design but were not implemented in the completed hackathon prototype.

## 4. My Ownership

PlantSight was a team project, so ownership is divided between individual implementation work and shared design decisions.

### I directly implemented

- Firebase integration across the application;
- user authentication and login functionality;
- the plant rating workflow;
- score calculation and score updates for residence halls;
- logic for displaying plants for users to rate;
- portions of the Google Maps integration.

### Shared with teammates

- overall application concept;
- campus competition concept;
- Google Maps functionality;
- general user-interface design.

### Teammate contributions

My teammates contributed to areas including:

- portions of the Google Maps implementation;
- image-upload functionality;
- social-media-style interactions;
- UI development;
- hackathon presentation and project communication.

The idea of using competition to encourage student participation was developed as a team. My primary contribution to that concept was implementing the underlying rating and scoring workflow.

## 5. Architecture / Data Flow

### High-Level Architecture

```text
Browser Application
        ↓
     Firebase
   ↙    ↓     ↘
Users  Plants  Hall Scores
        ↓
 Google Maps / UI
```

Firebase acts as the central application data layer, while Google Maps provides a visualization layer for participating residence halls.

### Rating Workflow

```text
User
  ↓
Select Residence Hall / Plant
  ↓
View Plant
  ↓
Choose Rating (1–5)
  ↓
Rating Logic
  ↓
Update Residence-Hall Score
  ↓
Firebase
  ↓
Updated Score Display
```

The score represents an aggregate value associated with the participating residence hall.

### Image Upload Workflow

```text
User
  ↓
Choose Image
  ↓
Add Associated Information
  ↓
Residence Hall / User / Plant Metadata
  ↓
Firebase Storage / Database
  ↓
Application Display
```

This is a **conceptual architecture** intended to explain the major application relationships rather than reproduce the exact Firebase database paths.

## 6. Key Technical Decisions

### Firebase

Firebase was selected because the team needed a backend that could be integrated quickly during a hackathon without building and deploying a separate server infrastructure.

It provided accessible libraries for storing application data and managing user-related functionality while allowing the team to focus on building the prototype.

### Google Maps

Google Maps was selected to provide a familiar geographic interface for showing participating residence halls across campus.

The map was primarily a visualization layer. Core score calculation was not dependent on the map itself, so rating data could still exist independently of the map display.

### Rating System

A 1-to-5 rating system was chosen because it provided a simple interaction that users could understand immediately during a short hackathon demo.

Ratings were used to contribute to residence-hall scores, supporting the project's competitive concept.

### Hackathon Trade-Off

The largest trade-off was **UI/UX versus functionality**.

Because development time was limited, the team prioritized implementing the application's core workflows—authentication, Firebase integration, mapping, rating, scoring, and content upload—over fully polishing the user interface and interaction design.

## 7. Reliability / Failure Behavior

The prototype currently has limited formal reliability handling.

### Google Maps Failure

If Google Maps fails to load, the map visualization is unavailable. However, the underlying rating and score data remains stored separately in Firebase.

### Firebase Failure

Firebase is a central dependency of the application. If Firebase is unavailable, most application functionality involving authentication, ratings, scores, and stored content becomes unavailable.

### Current Gaps

The hackathon prototype does not currently include:

- formal fallback behavior for Firebase outages;
- validation preventing all invalid rating or post submissions;
- a complete authorization model for determining which users are qualified to submit ratings;
- comprehensive Firebase Security Rules;
- automated dependency-failure tests.

Functional workflows were manually tested during development, but formal reliability testing and failure-injection testing were not part of the hackathon implementation.

## 8. Results / Project Scope

The team completed a working prototype containing the majority of the core concept, including:

- login functionality;
- Firebase-backed application data;
- Google Maps visualization;
- plant rating;
- residence-hall score updates;
- image uploads;
- user-generated content.

The application was demonstrated during the hackathon and received feedback during the judging/demo process.

The project did not produce a meaningful quantitative impact metric because it was developed as a short-duration hackathon prototype rather than deployed to a sustained user population.

The project includes information for more than **100 plant species**, although the original plan to supplement this information using external plant and weather APIs was not completed during the hackathon.

## 9. Limitations

Current limitations include:

- the rating workflow and interface require additional refinement;
- the application does not yet have a complete mechanism for determining which users are authorized to submit ratings;
- Google Maps has limited fallback behavior if the API is unavailable;
- Firebase represents a central application dependency;
- formal validation and security rules require additional work;
- the UI/UX remains at hackathon-prototype quality;
- OpenWeather integration was planned but not implemented;
- Perenual API integration was planned but not implemented;
- the project has not been deployed long enough to produce meaningful user-engagement or sustainability impact metrics.

## 10. Evidence

Available or planned evidence includes:

- public source-code repository;
- Google Maps implementation;
- Firebase integration code;
- authentication/login code;
- rating and score-calculation logic;
- sanitized screenshots;
- hackathon demo material;
- plant dataset containing 100+ species;
- team ownership table;
- conceptual architecture diagram.

Future evidence improvements include:

- a formal dependency/failure matrix;
- validation tests;
- security-rule documentation;
- sanitized Firebase data examples;
- clearer documentation of individual versus team ownership.
