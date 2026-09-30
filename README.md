# Phishing Awareness Simulation

A controlled phishing simulation built to study social-engineering attack patterns and measure real user susceptibility — not a theoretical writeup, an actual instrumented test run against a consenting group.

## Why this exists

Most phishing training is slides and quizzes. This project instead reproduces the real attack chain — lure email → credential-harvest page → behavioral logging — so the "awareness" part is backed by actual click-through and submission data instead of assumptions.

## Design decisions

**No real credentials are ever captured. ** The login form's input fields are disabled at the DOM level (`disabled` attribute), so even a user who tries to type gets nothing recorded. Only two events are logged: page opened and submit clicked — with a timestamp. This was a deliberate tradeoff: slightly less data than a "real" harvester, in exchange for the simulation being ethically clean to run against real people without legal/consent complications.

**Consent-first. ** No target receives the email without being told in advance that a phishing exercise is happening and agreeing to participate. This is a design constraint, not an afterthought — it's what separates this from an actual phishing attack.

**Static, framework-free.** Plain HTML/CSS/JS, hosted on GitHub Pages. No backend needed for the MVP — logging currently writes to the browser console and is designed to be swapped for a real endpoint (Google Apps Script / small serverless function) without touching the page markup.

## Attack chain

```
email-template.html   →   fake-login.html   →   awareness.html
 (lure: fake IT          (credential-harvest       (immediate debrief:
  password-expiry          lookalike page,           what happened, red
  notice)                  logs click only)          flags, countermeasures)
```

1. **`email-template.html`** — fake IT "password expiring in 24 hours" notice, styled to mimic an internal IT Service Desk email. Uses urgency and authority — two of the most effective phishing levers.
2. **`fake-login.html`** — a lookalike login page. Fields are disabled; only the click event is logged.
3. **`awareness.html`** — the moment a target clicks Sign In, they're redirected here instantly, told it was a simulation, and shown exactly which red flags they missed.

## Live pages

- Login page: `fake-login.html`
- Awareness/debrief: `awareness.html`
- Email source: `email-template.html`

## Metrics tracked

- Open rate (email link clicked)
- Submission rate (Sign In clicked on fake page)
- Time between email and click
- Self-reported vs. actual click behavior (cross-checked against consent/debrief form)

## Results

_To be filled in after the test group run — open rate, click rate, and any qualitative feedback from the debrief form._

## Countermeasures (from analysis)

- Verify sender domain, not just display name
- Treat "urgent" + "act within X hours" as a red flag by default
- Never authenticate through an emailed link — navigate to the service directly
- Report suspicious emails to IT/security rather than deleting silently

## Repo structure

```
email-template.html   - the lure
fake-login.html        - the harvest page (no real capture)
awareness.html          - post-click debrief
screenshots/             - test run evidence
```

## Ethics
This project was run only against informed, consenting participants, with no real credentials collected at any point. It exists to demonstrate the attacker's side well enough to build better defenses — not to be reused as an actual phishing tool.

## Author
Sakshi Sharma


This project was run only against informed, consenting participants, with no real credentials
collected at any point. It exists to demonstrate the attacker's side well enough to build better
defenses — not to be reused as an actual phishing tool.
