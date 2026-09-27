# Recovery & Exit Tracker — Offline App

A real native Android APK isn't something buildable in this environment (no Android SDK/Gradle
toolchain, no signing, no Play Store access) — but this is the closest real equivalent: a
**Progressive Web App (PWA)** that installs to your home screen, opens without any browser
chrome, runs fully offline after the first load, and stores everything in `localStorage`
**on your device only**. Nothing here ever leaves your phone unless you explicitly export it.

## Install on Android

A PWA needs to be loaded once over `https://` for the install prompt and offline caching to
work properly (opening the HTML file directly via `file://` will run it, but Android tends to
wipe `file://` storage unreliably between sessions — not good for a tracker you check daily).
Cheapest reliable path, since you already have GitHub:

1. Create a new GitHub repo (can be private), and push these four files/folders as-is:
   `index.html`, `manifest.json`, `service-worker.js`, `icons/`
2. In the repo settings, enable **GitHub Pages** (serve from the root of `main`)
3. On your Android phone, open the resulting `https://<you>.github.io/<repo>/` URL in Chrome
4. Tap Chrome's menu → **"Add to Home screen"** / **"Install app"**
5. Open it from the home screen icon from now on — it'll run standalone and offline

## Using it day to day

Same as before: check off today's recovery items and hit **Save today**, tick off exit
milestones as you hit them, log offers as they come in and the floor/strong/stretch verdict
is computed automatically.

## The analysis loop

**Export data** → downloads a `tracker-export-YYYY-MM-DD.json` file with your full history.
Send me that file in a chat whenever you want a review.

I'll read it and hand back an **analysis report** — a JSON file following the schema below.
**Import report** in the app reads that file, shows the summary/metrics/recommendations at the
top of the app, and can add, remove, or relabel checklist items if the analysis calls for a
change to the routine. Past reports stay listed underneath so you can see how things have moved
over time.

### Analysis report schema

```json
{
  "type": "analysis-report",
  "reportDate": "2026-10-05",
  "summary": "One or two sentences on where things stand.",
  "metrics": {
    "Adherence (30d)": "71%",
    "Best streak": "9 days",
    "Applications sent": "14"
  },
  "recommendations": [
    "Plain-language suggestion one.",
    "Plain-language suggestion two."
  ],
  "checklistChanges": {
    "recovery": {
      "add": [{ "id": "walk", "label": "10-minute walk after waking" }],
      "remove": ["nap"],
      "relabel": [{ "id": "wake", "label": "Held wake-time within 15 min" }]
    },
    "exit": {
      "add": [],
      "remove": [],
      "relabel": []
    }
  }
}
```

Every field is optional except `type` and `reportDate` — send just a `summary` for a quick
check-in, or the full thing with `checklistChanges` when the routine actually needs to change.
`metrics` is just a label → value map and renders as-is, so it can hold whatever's relevant
that round (adherence rate, streaks, application counts, interview counts, anything).
