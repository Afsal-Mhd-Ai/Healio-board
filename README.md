# ORBITAL

**Know your signal.**

An offline-first astronaut health-monitoring **prototype** for long-duration spaceflight scenarios. Built for the NASA Space Apps Challenge 2026: *Create Health Monitoring Software for Astronauts on Space Missions*.

> **Not a medical device.** All thresholds, scores, forecasts, simulated data, and recommendations are for demonstration only. ORBITAL cannot diagnose, treat, determine fitness for duty, contact a clinician, or replace mission procedures.

---

## Overview

A crew far from Earth may need to notice and communicate health changes before ground support can respond. ORBITAL lets a crew member:

- Record health indicators
- Compare them with logged trends
- See rule-based alerts in spaceflight-hazard context
- Prepare a local summary for a flight surgeon

### Hazard categories modeled

- Radiation
- Isolation and confinement
- Distance from Earth
- Altered gravity
- Hostile closed environments

### Mission modes

ISS / low Earth orbit, Orion transit, lunar base, Mars transit, Mars surface. The Mars modes show an illustrative **22-minute one-way communication delay**. This is a simulation, not a connection to NASA systems, a spacecraft, or a flight surgeon.

---

## Features

| Section | What it does |
|---|---|
| **Home** | Mission context, daily illustrative health score, active alerts, next measurement due, key indicators, mission-duration model for bone, muscle, and cardiovascular fitness |
| **Body** | Interactive SVG body map with 8 systems (brain/wellbeing, eyes, heart/circulation, lungs, bones/spine, muscles, immune/blood, gut/nutrition); shows associated hazard, recent readings, general context |
| **Trends** | One indicator at a time over 7 days, 30 days, or the full mission; personal reference band, flagged readings, hover details, optional simple early-warning estimate |
| **Check-in** | Records 14 indicators plus a separate wellbeing check-in (mood, sleep quality, isolation, crew conflict, workload, optional private note) |
| **Coach** | Log aerobic, resistive, or combined exercise; general hydration, nutrition, sleep, radiation-event, and wellbeing reminders |
| **Ask Doc** | Offline, button-selected answers from a small built-in rules/knowledge base; escalates urgent situations toward crew and mission procedures. Not a conversational AI |
| **Handoff** | 7-day summary of alerts, readings, and actions; copy or print. "Queue" is local only |
| **Mission** | Change environment mode, preview communication delay, scrub or play a synthetic 180-day mission, inject radiation / fever / missed-sleep / glucose events |
| **Sources** | Links to public NASA, ESA, JAXA, and CSA material (these do not validate the app's thresholds) |
| **Settings** | Edit and restore demo thresholds, export data as JSON, clear data, hide private notes in history |

### Tracked indicators (14)

Heart rate, systolic BP, diastolic BP, blood glucose, temperature, SpO₂, sleep duration, mood, stress, hydration, exercise, radiation dose, body mass, energy intake.

---

## How the logic works

All thresholds, units, ranges, and source notes live in the `CONFIG` object near the top of the inline script in `index.html`.

1. **Boundary check:** each reading is classified as nominal, caution, or warning. Missing readings show as **No data**, never assumed healthy.
2. **History comparison:** rolling 7-day mean and mean of prior logged readings.
3. **Explainable alerts:** each alert states what is happening, why it matters in space, and what to do now, with triggering rule, linked hazard, confidence label, and escalation label.
4. **Mode weighting:** mission mode hazard weights affect alert ordering and emphasis only. No clinically validated mode-specific thresholds.
5. **Demo score:** starts at 100, minus 24 per red alert, minus 10 per yellow alert, plus up to 6 for recent records, bounded 0 to 100. Illustrative only.
6. **Trend projection:** with 3 or more readings in the last 14 days and a detectable linear slope, estimates when a caution boundary might be reached. Rough and uncertain, with no confidence interval.

---

## Quick start

No install, no build step.

```bash
git clone https://github.com/Afsal-Mhd-Ai/orbital-astronaut-health.git
cd orbital-astronaut-health
# open index.html in a current browser
```

On first run, step through or skip the briefing.

> **Note:** the Check-in form is prefilled with illustrative example values. Update them before saving, or the examples will be recorded as real entries.

---

## Demo walkthrough (~3 minutes)

1. Open `index.html` and skip or step through the first-run briefing.
2. **Mission** → select **Mars transit**.
3. Select **Load 180-day scenario**. This creates 180 synthetic daily records containing:
   - Radiation spike: days 73–75
   - Sleep disruption: days 92–99
   - Exercise gap: days 120–131
   - Glucose drift: from about day 133
   - Mood dip: days 146–158
4. Move the timeline slider near day 73, then open **Home** to see the resulting alerts. Or use the event-injection buttons on the Mission page.
5. Inspect an alert's three parts (what / why in space / what to do) and its hazard, rule, confidence, and escalation labels.
6. **Body** → select a system (e.g. heart/circulation) → **Trends** → switch indicator and range.
7. **Handoff** → review the 7-day summary, copy or print.
8. **Mission** → **Reset scenario** (removes simulator and injected records; manual entries are kept).

---

## Tech stack

A single self-contained HTML file.

- **HTML:** page shell, navigation, forms, modal
- **CSS (embedded):** dark/light themes, responsive layout, bottom bar on phones, reduced-motion support
- **Vanilla JavaScript:** config, state, persistence, rules engine, simulator, views
- **Inline SVG:** score gauge, charts, icons, body map

No React, package manager, external scripts or styles, build pipeline, server, or API key.

Browser features used: `localStorage` (with in-memory fallback), SVG, clipboard, print.

---

## Data and privacy

- Data is stored in the browser's `localStorage` under the key `orbital-state`.
- If storage is unavailable, the session falls back to memory and shows a notice.
- No backend, accounts, network API, or sensor integration. The app makes no network requests (only the Sources links need internet).
- Local storage is **not encrypted**. Anyone with access to the browser profile may read the data.
- "Hide private notes" is a display preference only. It does not erase or secure notes.

---

## Testing

Run in the browser console:

```js
window.orbitalSelfTest()
```

Four built-in checks: nominal heart rate, temperature caution, low-SpO₂ warning, low-mood wellbeing alert.

Manual smoke checks have verified that all 10 views render and the simulator generates 180 records and responds to event injection.

---

## Limitations

ORBITAL does **not**:

- Ingest wearable or medical-device data
- Validate diagnoses
- Use a trained predictive model
- Encrypt health records or authenticate users
- Connect to NASA / ESA / JAXA / CSA systems
- Send data to a flight surgeon or sync its handoff queue

Score, mission twin, hazard mappings, thresholds, forecasts, reminders, and simulator are all demonstration logic. Operational use would require clinical and human-factors review, approved mission protocols, validated data and thresholds, security and privacy design, and appropriate certification.

---

## Roadmap

- Validated thresholds with clinical review
- Encrypted local storage
- Wearable and sensor data ingestion
- Secure crew-to-ground handoff sync
- Strict mission-start baseline filtering
- Expanded automated tests

---

## Author

**Afsal Muhammed P S** (Loki Afsal), Government Engineering College Idukki
GitHub: [Afsal-Mhd-Ai](https://github.com/Afsal-Mhd-Ai)
