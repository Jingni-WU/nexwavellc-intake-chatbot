# Nexwave Project Intake Assistant

A browser-based chatbot that collects project requirements from prospective Nexwave LLC clients and returns a preliminary design approach and timeline estimate.

**Live demo:** `https://<your-github-username>.github.io/nexwave-intake-chatbot/`

<!-- Add a screenshot after deploying: ![Screenshot](screenshot.png) -->

## Why this exists

Early sales conversations usually spend the first meeting gathering the same basic information: who the users are, where the data lives, what the first release needs, and what security rules apply. This assistant asks those questions up front, in a structured way, so that:

- clients get an immediate, realistic sense of scope and timing, and
- the Nexwave team starts the first call with a completed brief instead of a blank page.

## What it asks

The conversation covers six areas, matching the questions Nexwave uses in discovery:

| Area | Questions |
|------|-----------|
| Project type | Web application, customer portal, internal tool, mobile app, or integration |
| Users | Who uses the application, and what can each role do? |
| Data | Which system remains the source of truth? Which systems must be connected? |
| First release | What is essential in the first useful release? |
| Security | Are there security, compliance, or record-retention requirements? |
| Timing and contact | Target launch window, client name and email |

Every question accepts either a quick-select option or free text. Free-text answers are scanned for keywords (for example "Salesforce", "HIPAA", "SSO", "approval"), which are added to the brief automatically.

## What it produces

After the last question the assistant generates a proposal card containing:

- **Recommended architecture**: stack, hosting, access control, data ownership and sync approach, audit and retention handling, security baseline
- **Roles and access**: a starting permission table for each selected role
- **First release scope**, plus candidate features for phase 2
- **Timeline**: five phases (discovery, design, build, testing, launch) with a total range in weeks
- **Estimate drivers**: a line-by-line breakdown of what added time
- **Suggested team** size
- A warning when the client's target date is shorter than the estimate, with trade-off options

Clients can copy the brief as plain text or print / save it as a PDF. A live "Project brief" panel on the right fills in as the client answers (on mobile it opens from the "View brief" button).

## How the estimate works

The estimate is rule-based and fully transparent. It starts from a baseline for the project type and adds weeks for each factor:

| Factor | Weeks added |
|--------|-------------|
| Project type baseline | 4–9 (integration 4, internal tool 5, web app 6, portal 7, mobile 9) |
| Each user role beyond two | +0.75 |
| Source of truth is an existing CRM/ERP | +1.5 |
| Each system integration | +1.5 |
| First-release features | +0.5 to +2 each (e.g. approvals +2, payments +2, dashboards +1.5) |
| Security and compliance | SSO +1, audit log +1, HIPAA +3, SOC 2 +2, PCI +2 |
| Record retention | +0.5 to +1.5 |

Build time is then wrapped with discovery (1.5–3 weeks), design (1–1.5 weeks), testing (20% of build, minimum 1 week), and launch (1 week). The final range is the total × 0.85 to × 1.2.

All weights are defined as constants at the top of the script in `index.html` (`TYPES`, `ROLES`, `SOT`, `SYSTEMS`, `FEATURES`, `SECURITY`, `RETENTION`, `TARGET`). They are starting assumptions and should be calibrated against Nexwave's past projects.

## Tech

- A single self-contained `index.html` file: HTML, CSS, and vanilla JavaScript
- No build step, no framework, no backend, no dependencies (Google Fonts only, with system-font fallback)
- Responsive down to mobile, light and dark mode, keyboard accessible, respects reduced-motion settings
- No client data leaves the browser in the current version

## Run locally

Open `index.html` in any modern browser. That's it.

## Deploy with GitHub Pages

1. Push this repository to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)`, then save.
4. After a minute or two the site is live at `https://<your-github-username>.github.io/<repo-name>/`.
5. Update the live demo link at the top of this README.

## Project structure

```
nexwave-intake-chatbot/
├── index.html   # the entire app: layout, styles, question flow, estimate logic
└── README.md
```

Inside `index.html`, the script is organized into sections: question flow configuration, state, brief panel rendering, estimate calculation, proposal rendering, and composer (input) handling. To add or change a question, edit the `steps` array.

## Roadmap

- **Send briefs to Nexwave**: submit completed briefs to a sales inbox or CRM (for example via Formspree, a small serverless function, or a HubSpot form endpoint)
- **Calibrate estimates** with historical project data from Nexwave
- **AI-written summaries**: call a language model from a small backend to turn free-text answers into a polished narrative (API keys must stay on the server, never in this front-end code)
- **Cost ranges** alongside the timeline
- **Multi-language support**

## Disclaimer

Estimates produced by this tool are automated first-pass figures. A Nexwave consultant confirms scope, timeline, and cost after a discovery call.

## Author

Built by `<your name>` as an internship project at Nexwave LLC, 2026.
