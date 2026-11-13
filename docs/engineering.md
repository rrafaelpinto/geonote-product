# Engineering decisions

A selection of decisions that moved GeoNote beyond a basic prototype. These describe implemented directions and their trade-offs, rather than hypothetical architecture.

## 1. Prefer operating-system geofencing to persistent polling

**Problem:** background execution is unreliable and power-sensitive on mobile devices. A constantly running location stream is a poor default for place-triggered reminders.

**Decision:** use Android's native `GeofencingClient` integration and local notification delivery, with persisted device-side monitoring state and explicit re-entry/cooldown behavior.

**Trade-off:** the operating system controls delivery timing and imposes limits. The implementation monitors a bounded set of reminders (up to 50 in the documented Android policy), so coverage and permission state must be visible to the user.

**Evidence:** the project documents successful physical Android device tests, including notification after the application process ended and restoration after device reboot. Those tests do not establish universal reliability across manufacturers or OS versions.

## 2. Use PostgreSQL with PostGIS for spatial concerns

**Problem:** reminders and public discovery depend on geographic locations, not just strings or addresses.

**Decision:** store relational state in PostgreSQL with PostGIS geospatial support. Treat private reminder triggering and nearby public discovery as separate operations.

**Trade-off:** explicit geographic filtering, visibility rules, and API boundaries add complexity, but they keep personal data access and social discovery behavior understandable.

## 3. Version the API for independently deployed clients

**Problem:** the backend can deploy much more frequently than mobile applications update.

**Decision:** make `/api/v1/` the canonical mobile contract. Favor compatible changes within the version and require a deliberate version change or compatibility strategy for breaking changes.

**Trade-off:** compatibility tests and release identity discipline become engineering requirements rather than optional cleanup tasks.

## 4. Keep shared reminder operations deterministic

**Problem:** retries and concurrent actions can create duplicate copies or inconsistent analytics in a sharing workflow.

**Decision:** enforce copy uniqueness in the data layer, make copy creation transactional and idempotent for the supported workflow, and preserve provenance.

**Trade-off:** explicit domain invariants and concurrency handling add implementation effort, but avoid relying exclusively on client-side checks.

## 5. Add guardrails before expanding public content

**Problem:** public or link-shared reminders introduce privacy and moderation responsibilities absent from purely personal reminders.

**Decision:** define private, unlisted, and public visibility semantics; introduce publication safeguards, user reporting, administrative enforcement, and an audit trail.

**Trade-off:** moderation infrastructure grows with product surface area. Human/device operational validation remains separate from successful automated tests.

## 6. Treat release engineering as part of product engineering

**Problem:** a working debug build is not evidence that an app can be shipped reproducibly.

**Decision:** use CI checks, Android signing controls, release metadata, and explicit version/build identification. Record tests and physical-device smoke validation instead of equating a merged PR with a completed release.

**Trade-off:** some checks, including visual E2E and cross-device scenarios, still require additional operational validation before public distribution.

[Back to overview](../README.md) · [Architecture](architecture.md) · [Release status](release-status.md)
