# GeoNote

**Reminders for where life happens.**

GeoNote is a location-aware reminder product built to turn real-world places into useful reminders. Instead of scheduling every task for a specific time, users can attach an action to a place and receive a notification when they arrive nearby.

The mobile MVP is implemented and has been validated on a physical Android device. Its backend and landing page are deployed. **Public app-store distribution has not started.**

**[Product website](https://geonote.net/)** · [Product experience](docs/product.md) · [System architecture](docs/architecture.md) · [Engineering decisions](docs/engineering.md) · [Release status](docs/release-status.md)

## The product

A user can save a reminder such as "Pick up a prescription" and associate it with a pharmacy. GeoNote uses a configurable geographic radius to determine when the reminder is relevant, then relies on Android's native geofencing facilities to deliver a local notification.

The core experience includes:

- Create, edit, pause, resume, and delete location-based reminders.
- Choose a location using search, a map, or the device's current position.
- Browse reminders in list and map views.
- Receive proximity notifications without keeping the Flutter app continuously running.
- Sign in with a one-time email code and synchronize reminders with a backend.
- Share reminders with private, unlisted, or public visibility.
- Discover nearby public reminders from other users in a location-based feed.
- Manage account information, location permissions, and notification readiness.
- Use the interface in English or Brazilian Portuguese.

The location-based feed is intentionally separate from personal reminder alerts: public discovery and private reminder delivery solve different problems and follow different rules.

## From concept to working software

```text
Product definition → Flutter mobile MVP → Django/PostGIS API
          → Android background validation → CI and release workflow
          → Deployed backend + public website → Public launch preparation
```

GeoNote is not a UI concept or an unimplemented architectural proposal. The repository behind the product includes a Flutter application, a Django REST API, a spatial database model, native Android geofencing integration, automated checks, and release tooling. The Android app has also been installed and exercised on a real device.

**The distinction:** the backend and public website are deployed; Android mobile distribution is still in the pre-launch phase. An iOS release is not currently supported.

## Engineering at a glance

| Area | Implementation |
| --- | --- |
| Mobile client | Flutter / Dart with native Android geofencing integration |
| API | Python 3.12, Django, Django REST Framework |
| Data | PostgreSQL and PostGIS |
| Authentication | Email one-time codes and JWT sessions |
| API compatibility | Versioned mobile contract (`/api/v1/`) |
| Delivery | GitHub Actions, test automation, signed Android release workflow |
| Infrastructure | Render backend, static landing-page hosting |
| Observability | Structured logs, request correlation, HTTP metrics |

The public architecture diagram and the rationale behind significant decisions are available in [System architecture](docs/architecture.md) and [Engineering decisions](docs/engineering.md).

## What was technically difficult

**Reliable background reminders.** Mobile operating systems impose strict background execution limits. GeoNote uses native Android geofencing rather than a permanently active Flutter location stream, with explicit policies for re-entry and duplicate alerts. Physical-device validation included notifications after process termination and device reboot.

**Spatial data with clear boundaries.** PostgreSQL/PostGIS supports geographically anchored reminders. The API separates personal content, link sharing, and nearby public discovery so each feature has its own visibility and eligibility rules.

**A mobile-compatible backend.** Backend deploys and installed mobile versions move at different speeds. GeoNote uses a versioned REST contract, controlled release identifiers, and compatibility-oriented development to reduce that coupling.

**Safety and predictable state changes.** Shared content includes publication controls, moderation paths, auditability, and transactional handling for copies of shared reminders.

These are implemented engineering concerns, not claims of performance or scale that have not been independently measured. See [Engineering decisions](docs/engineering.md).

## Current availability

| Surface | Status |
| --- | --- |
| [Landing page](https://geonote.net/) | Public |
| Backend API | Deployed; app-facing service |
| Android app | Functional MVP; signed build and physical-device testing documented |
| Google Play | Not publicly released |
| iOS app | Not supported yet |

For boundaries and remaining validation, see [Release status](docs/release-status.md).

## About this repository

This is the **public product and engineering overview** for GeoNote. It is deliberately separate from the private implementation repository. It documents what has been built, why key design decisions were made, and what remains before broad distribution.

It does **not** contain the application source code, credentials, production configuration, or private delivery backlog. It is not an open-source distribution or a template.

**Built and maintained by Rafael Pinto.**

© 2026 Rafael Pinto. All rights reserved.
