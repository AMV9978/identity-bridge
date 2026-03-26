# Identity Bridge

> Unifying physical identity (OnGuard PACS) with digital identity (SSO/Active Directory) for SOC analysts.

**[Live Demo →](https://angelovasquez.net/identity-bridge/)**

---

## The Problem

When a SIEM alert fires, it contains a username — `jsmith` — and little else. The analyst then manually cross-references directories, badge systems, and HR records to answer one basic question: *who is this person, and where are they right now?*

The physical access control system already holds the answer. LenelS2 OnGuard stores full name, department, badge ID, access tier, photo, and last physical location for every cardholder. The data exists — it just lives in a separate silo, accessible only by clicking through a UI one record at a time.

Identity Bridge closes that gap.

---

## What It Does

Three integrated layers that work together in real time:

**Identity Resolution**
Resolve any SSO username, real name, or badge ID to a unified identity record — pulling cardholder data from OnGuard and account data from Active Directory in a single query.

**Alert Enrichment**
SIEM alerts are automatically enriched with physical context. When an alert fires on `jsmith`, analysts instantly see where that person last badged in, their access tier, department, and a plain-language recommendation — without leaving the dashboard.

**Anomaly Detection**
Cross-correlates physical badge events with digital login events to surface high-signal flags:
- Impossible travel (VPN login with no badge entry)
- Active sessions with no physical presence
- Repeated auth failures while physically on-site
- Badge/SSO time window mismatches

---

## Architecture

```
OnGuard (OpenAccess API)  ←→  Identity Bridge Middleware  ←→  AD / LDAP
                                        ↓
                              SIEM / SOC Dashboard
                         (enriched alerts + anomaly feed)
```

The middleware is a lightweight layer that requires no modification to existing OnGuard or SIEM infrastructure — read-only access to both systems is sufficient for the resolution and enrichment layers.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Physical identity | LenelS2 OnGuard OpenAccess REST API |
| Digital identity | Active Directory / LDAP / Azure AD |
| Middleware | Python |
| Anomaly engine | Rule-based cross-correlation |
| SIEM integration | Splunk / generic webhook |
| Frontend demo | HTML, CSS, JavaScript |

---

## Status

**Concept prototype — pending production API access.**

The interactive demo runs on realistic mock data modeled after the actual OnGuard cardholder schema and SOC alert patterns. Production deployment is gated on:

- OnGuard upgrade to v8.2 / 8.3 (in progress)
- OpenAccess API license secured
- AD/SSO source confirmed

---

## Running the Demo Locally

No build step or dependencies required.

```bash
git clone https://github.com/AMV9978/identity-bridge.git
cd identity-bridge
open index.html
```

Or just visit the **[live demo](https://angelovasquez.net/identity-bridge/)**.

---

## Background

This project started as a brainstormed concept with a SOC team encountering this exact identity gap daily. OnGuard's alarm monitoring UI already surfaces name, SSO, photo, and badge timestamp on every swipe — the data linkage exists, it's just locked inside a click-to-view interface rather than being available programmatically for enrichment or correlation.

The upgrade to OnGuard 8.x unlocks the OpenAccess API bulk export and event subscription features that make real-time enrichment feasible.

---

## Related Projects

- [SOC RAG Assistant](https://github.com/AMV9978) — RAG-powered chatbot for SOC triage guidance
- [Microsoft Azure SIEM Build](https://angelovasquez.net) — Cloud SIEM on Azure Sentinel with KQL alerting
- [Portfolio](https://angelovasquez.net) — angelovasquez.net

---

*Built by [Angelo Vasquez](https://angelovasquez.net) — M.S. Cybersecurity, NYU · Cybersecurity Engineer · AI Builder*
