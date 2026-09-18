# Mobile Application Penetration Testing — BSQ Mobile (Academic Project)

> Vulnerability Assessment conducted as part of the *Mobile Penetration Testing* course, Group 4. Target: an internal Android application used to manage dormitory operations at a university.
>
>**Note on scope:** This repository contains a sanitized, high-level summary only. The full technical report (including exact endpoints, tokens, and proof-of-concept scripts) is **not published here**, since the target application was live/in production at the time of testing and some findings may still be unremediated. Sharing exploitable details publicly could put real users' data at risk. The complete report is available privately on request (e.g. for academic or recruiting purposes).

## Overview

Our team of 5 carried out a black-box / static-analysis security assessment on an Android application (React Native + Hermes bytecode) used to support dormitory operations for a university residence. We worked collaboratively across the whole pipeline — reverse-engineering the APK, testing the backend API, and scoring/documenting findings — rather than splitting strictly by finding, so all of us touched most parts of the process.

The goal was to identify vulnerabilities that could compromise user data, backend integrity, or service availability, and to recommend remediations following industry standards.

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

## What I Took Away From This

The biggest lesson for me was how little "hidden" actually means in a mobile app — a token or key buried in compiled bytecode still gets pulled out in minutes with the right tools, and the most damaging finding (F004) wasn't some exotic exploit, it was a basic design decision (one encryption key shared across every install) that quietly broke the whole security model. It changed how I think about client-side vs. server-side trust, and gave me a much more concrete feel for what "defense in depth" actually looks like in practice — which is a big part of why I'm drawn to SOC/blue team work.

## Skills Demonstrated

- Android APK reverse engineering & static analysis
- React Native / Hermes bytecode decompilation
- REST API security testing
- Vulnerability scoring with CVSS v4.0
- Security report writing aligned with industry findings/remediation format

## Team

Group project of 5 — Mobile Penetration Testing coursework, BINUS University. We worked closely together across the full process rather than splitting into isolated tracks.

---
*This report was produced for academic purposes. Findings were responsibly limited to non-destructive testing; no user data was collected, retained, or misused during the assessment.*
