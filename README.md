# Nexwave Project Intake Assistant

A browser-based chatbot that collects project requirements from prospective Nexwave LLC clients and returns a preliminary design approach and timeline estimate.

**Live demo:** https://jingni-wu.github.io/nexwavellc-intake-chatbot/

## Overview

Early sales conversations at a software consultancy tend to spend the entire first meeting gathering the same basic information: who the users are, where the data lives, what the first release needs, and what security rules apply. The Nexwave Project Intake Assistant moves that step before the meeting. Clients answer a short guided conversation and immediately receive a structured brief with a recommended architecture and a realistic timeline range, and the Nexwave team starts the first call with a completed brief instead of a blank page.

The tool is complete and deployed as a static site on GitHub Pages. It requires no server, no installation, and no account.

## Features

**Guided requirements conversation.** The assistant walks the client through six areas that mirror Nexwave's discovery questions:

| Area | What the client is asked |
|------|--------------------------|
| Project type | Web application, customer portal, internal tool, mobile app, or integration |
| Users | Who uses the application, and what can each role do? |
| Data | Which system remains the source of truth, and which systems must be connected? |
| First release | What is essential in the first useful release? |
| Security | Are there security, compliance, or record-retention requirements? |
| Timing and contact | Target launch window, name and email |

Each question accepts a quick-select option or free text. Free-text answers are scanned for keywords (for example "Salesforce", "HIPAA", "SSO", "approval") and the matches are added to the brief automatically, so clients who prefer to type are not penalized.

**Live project brief.** A side panel fills in as the client answers, with a progress bar. On mobile it opens from a "View brief" button.

**Preliminary proposal.** After the last question the assistant generates a proposal containing a recommended architecture (stack, hosting, access control, data ownership and sync, audit and retention handling, security baseline), a starting permission table for each role, the first-release scope with phase-two candidates, a five-phase timeline with a total range in weeks, a line-by-line breakdown of what drives the estimate, and a suggested team size. When the client's target date is shorter than the estimate, the proposal says so and offers trade-off options.

**Export.** Clients can copy the brief as plain text or print / save it as a PDF.

**Quality.** Responsive down to phone screens, light and dark mode, keyboard accessible, and respects reduced-motion settings. No client data leaves the browser.

## How the estimate works

The estimate is rule-based and fully transparent, so the sales team can explain every number to a client. It starts from a baseline for the project type and adds weeks for each factor:

| Factor | Weeks added |
|--------|-------------|
| Project type baseline | Integration 4, internal tool 5, web app 6, portal 7, mobile 9 |
| Each user role beyond two | +0.75 |
| Source of truth is an existing CRM/ERP | +1.5 |
| Each system integration | +1.5 |
| First-release features | +0.5 to +2 each (approvals +2, payments +2, dashboards +1.5) |
| Security and compliance | SSO +1, audit log +1, HIPAA +3, SOC 2 +2, PCI +2 |
| Record retention | +0.5 to +1.5 |

Build time is then wrapped with discovery (1.5–3 weeks), design (1–1.5 weeks), testing (20% of build, minimum one week), and launch (one week). The final range is the total multiplied by 0.85 and 1.2.

All weights are constants at the top of the script in `index.html` (`TYPES`, `ROLES`, `SOT`, `SYSTEMS`, `FEATURES`, `SECURITY`, `RETENTION`, `TARGET`), so they can be recalibrated without touching the rest of the code.

## Design decisions

**Rule-based estimates instead of a language model.** A generated estimate would be hard to explain and could vary between identical inputs. Fixed, visible weights make every proposal reproducible and let Nexwave staff adjust the numbers from their own project history.

**Guided questions with free-text fallback.** Quick-select options keep answers structured enough to estimate from, while free text lets clients describe unusual needs in their own words.

**Single static file.** Keeping everything in one `index.html` with no dependencies or backend means it can be hosted anywhere for free, reviewed in one place, and embedded on the company website later without an engineering handoff.

**First release framed as "what users can't work without."** Clients tend to list every feature they can think of. The wording of that question, and the automatic phase-two list, steer the conversation toward a realistic first release.

## Development process

The project moved through five stages:

1. **Requirements.** Defined the discovery questions the assistant needed to cover and the information the sales team wanted in hand before a first call.
2. **Conversation design.** Wrote the question flow, answer options, and wording, including conditional steps (for example, the integrations question only appears when an external system is the source of truth).
3. **Estimation model.** Built the weighted estimate and the five-phase timeline, and tested it against a range of project profiles, from a small internal dashboard to a multi-integration HIPAA portal, to check that results stayed plausible.
4. **Interface.** Built the chat interface, live brief panel, and proposal card, then added mobile layout, dark mode, keyboard access, copy and print export.
5. **Deployment and documentation.** Published on GitHub Pages and wrote this README.

## Tech stack

HTML, CSS, and vanilla JavaScript in a single file. No framework, build step, backend, or third-party libraries (Google Fonts only, with a system-font fallback).

## Run locally

Open `index.html` in any modern browser.

## Project structure

```
nexwavellc-intake-chatbot/
├── index.html   # the entire app: layout, styles, question flow, estimate logic
└── README.md
```

Inside `index.html` the script is organized into sections: question flow configuration, state, brief panel rendering, estimate calculation, proposal rendering, and input handling. Questions are defined in the `steps` array.

## Development Log

A week-by-week record of the project's development from July to September 2026, covering requirements analysis, conversation design, estimation logic, implementation, testing, and deployment.

[View the full development log →](development-log.md)

## Author

Jingni Wu, summer project at Nexwave LLC, 2026.
