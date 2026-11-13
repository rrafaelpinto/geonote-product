# Release status

**Public overview prepared:** November 13, 2026  
**Evidence basis:** GeoNote private repository on its default branch and its recorded engineering documentation. Runtime monitoring and app-store approval were not independently verified as part of this document.

## Shipped infrastructure vs. mobile availability

| Area | Evidence-backed state | Boundary |
| --- | --- | --- |
| Product website | Published at [geonote.net](https://geonote.net/) | Public product information, not a mobile app download |
| Backend | Deployment configuration and public service URL documented | Live uptime and configuration not independently audited here |
| Android MVP | Implemented; signed APK workflow and device smoke tests documented | Broader device compatibility and public store release remain pending |
| Background notifications | Physical-device observations include closed-process alerts and recovery after reboot | Timing, battery use, and every OS/device scenario are not guaranteed |
| Public sharing and feed | Implemented in the application and API | Operational/moderation validation remains relevant before broad rollout |
| iOS | Not supported as a release target in the documented implementation | No native iOS host, signing, or device validation documented |
| Visual E2E | Automation foundation exists | Comprehensive authenticated flows and stable device-matrix execution remain incomplete |

## Release interpretation

The product has moved from design and prototype work to an implemented MVP with a deployed backend, a public landing page, and an Android build validated on physical hardware. It should **not** be described as generally available on Google Play or the App Store.

Successful automated test suites and signed artifacts support release readiness, but they do not replace mobile device verification, store review, observability, or operational rollout checks.

## What this repository is not

This repository is a public engineering and product presentation. It contains no application source or production configuration and does not grant an open-source license. Contact the owner for additional technical discussion.

[Back to overview](../README.md)
