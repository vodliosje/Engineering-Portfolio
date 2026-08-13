# Engineering RFC — PlantSight Hardening Without a Rewrite

## Decision

Keep PlantSight as a hackathon project and add a **small reliability/security hardening slice**. Do not rebuild it into a new production application solely for portfolio purposes.

## Goals

- prove the rating/scoring logic is correct;
- test authorization boundaries;
- expose concurrent-update behavior;
- handle Firebase and Google Maps failures visibly;
- make setup reproducible;
- preserve clear individual/team ownership.

## Non-Goals

- redesigning the entire UI;
- implementing every planned API;
- claiming production scale;
- rewriting Firebase into a custom backend;
- retroactively presenting OpenWeather/Perenual as hackathon features.

## Highest-Risk Technical Slice

The residence-hall score is shared mutable state. The most valuable reliability question is whether two users rating at nearly the same time can create a lost update or inconsistent aggregate.

Test this before adding new features.

## Required Tests

1. rating below 1 → reject;
2. rating above 5 → reject;
3. valid rating → accepted and score changes as expected;
4. known fixture ratings → expected aggregate score;
5. unauthenticated write → reject where auth is required;
6. user A cannot modify protected user B data where ownership rules apply;
7. two near-simultaneous score updates → no lost update;
8. Firebase write failure → UI does not report false success;
9. Maps unavailable → explicit fallback/error state rather than unexplained blank behavior.

## Security Decision

Review Firebase Security Rules and any public client/API configuration. Public browser configuration is not automatically a secret, but unrestricted credentials or overly broad database rules create risk.

Expected controls:

- minimum required read/write permissions;
- validation at the data-rule boundary where feasible;
- origin/API restrictions for map credentials where applicable;
- no private service credentials committed to the repository.

## Success Criteria

- a clean environment can start the application;
- rating/auth tests run automatically;
- concurrent-update behavior is known and fixed if necessary;
- failed writes are visible and do not create false-success UI state;
- source contains no private credentials;
- ownership remains explicit in public documentation.
