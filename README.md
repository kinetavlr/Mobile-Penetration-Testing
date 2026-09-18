# Mobile Application Penetration Testing — BSQ Mobile (Academic Project)

> Vulnerability Assessment conducted as part of the *Mobile Penetration Testing* course, Group 4. Target: an internal Android application used to manage dormitory operations at a university.
>
> **Note on scope:** This repository contains a sanitized, high-level summary only. The full technical report (including exact endpoints, tokens, and proof-of-concept scripts) is **not published here**, since the target application was live/in production at the time of testing and some findings may still be unremediated. Sharing exploitable details publicly could put real users' data at risk. The complete report is available privately on request (e.g. for academic or recruiting purposes).

## Overview

This project was a black-box / static-analysis security assessment of an Android application (React Native + Hermes bytecode) used to support dormitory operations for a university residence. The goal was to identify vulnerabilities that could compromise user data, backend integrity, or service availability, and to recommend remediations following industry standards.

## Methodology

- **Static analysis / reverse engineering** of the APK: decompiling DEX bytecode and inspecting extracted resources to identify hardcoded secrets and insecure logic.
- **Bytecode decompilation** of the app's React Native/Hermes bundle to recover readable application logic.
- **API testing**: manual and scripted testing of backend endpoints to check authentication/authorization enforcement.
- **CVSS v4.0 scoring** of each finding to standardize severity and prioritize remediation.

## Tools Used

| Tool | Purpose |
|---|---|
| JADX | Decompiling DEX bytecode to Java |
| Apktool | Decompiling the APK and extracting resources |
| hermes-dec | Decompiling React Native Hermes bytecode |
| `strings` / `grep` | Searching for hardcoded string values |
| `curl` / Python `requests` | API endpoint testing |
| Python scripts | Automated API test cases |

## Findings Summary

| ID | Severity | Category |
|---|---|---|
| F001 | High | Sensitive user data exposed via an over-permissive, hardcoded cloud storage link (CWE-200) |
| F002 | Medium | Hardcoded third-party service secret token embedded in the client app (CWE-798) |
| F003 | Medium | Missing authorization checks on backend API endpoints (CWE-639) |
| F004 | Critical | Hardcoded, shared encryption key used across all app installs (CWE-321) |

*(Exact locations, tokens, and endpoints are withheld from this public summary.)*

### Key Takeaways

- **Client-side secrets are not secrets.** Anything shipped inside an APK — API tokens, encryption keys, backend URLs — can be recovered by an attacker with basic reverse-engineering tools. Secrets belong server-side only.
- **A single shared encryption key defeats the purpose of encryption.** When every install of an app uses the same key, an attacker who extracts it from one copy can decrypt or forge traffic for *every* user.
- **Authorization must be enforced server-side**, not assumed from client behavior — endpoints without server-side authorization checks are exploitable regardless of what the UI allows.
- **Cloud storage/CDN links should never double as access control.** A "hidden" but unauthenticated link is not private if anyone with the link (or an active account in a shared tenant) can browse it.

## Skills Demonstrated

- Android APK reverse engineering & static analysis
- React Native / Hermes bytecode decompilation
- REST API security testing
- Vulnerability scoring with CVSS v4.0
- Security report writing aligned with industry findings/remediation format

## Team

Group project — Mobile Penetration Testing coursework, BINUS University.

---
*This report was produced for academic purposes. Findings were responsibly limited to non-destructive testing; no user data was collected, retained, or misused during the assessment.*
