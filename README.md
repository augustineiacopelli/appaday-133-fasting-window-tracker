# AppADay 133: Fasting Window Tracker

**Live:** https://augustineiacopelli.github.io/appaday-133-fasting-window-tracker/

**Category:** Health (H) | **Shipped:** 2026-09-17 | **Part of** [AppADay](https://augustineiacopelli.github.io/appaday/)

Track an intermittent fasting window on a single 24 hour ring. Pick 16:8, 18:6 or 20:4, start the fast, and the ring fills from your start position clockwise around a clock face where midnight sits at the top. A live countdown, a projected end marker, and a current time hand keep the whole day legible at a glance.

## What it does

The ring is a real 24 hour dial rather than a generic progress circle, so a fast that begins at 8pm and ends at noon the next day draws as two arcs that meet at the top of the face. The center reads out the state, a running H:MM:SS countdown, the projected end time, and percent complete. When the target is reached the fast does not auto end. The ring holds at full and the readout flips to counting up past target, because ending the fast is a decision you make, not something a timer makes for you.

Ending before target is logged in grey as an early end and counted in your history. The interface never calls it a failure.

Below the ring: current streak, longest streak, and completed fasts this month. A month calendar colors each logged day teal for a completed fast and grey for an early end, and tapping a day expands the detail below the grid. A reverse chronological history lists every fast with its clock times, achieved duration against target, and controls to edit or delete it.

## Notes on the build

Every countdown and clock figure uses tabular numerals so nothing shifts as the digits tick. A single one second interval drives the whole display, and every frame recomputes elapsed time from `Date.now()` against the stored start timestamp rather than decrementing a counter, so backgrounding the tab, locking the phone, or sleeping the laptop cannot drift the clock. Rendering also rebinds to `visibilitychange`, `pageshow`, and `focus`.

The start time of an active fast is editable, clamped to no later than now and no earlier than 48 hours ago, for the mornings you remember at 9am that you actually stopped eating at 7pm.

Data lives in `localStorage` under `appaday133-active`, `appaday133-history`, and `appaday133-prefs`, with every read and write wrapped in try and catch and an in memory fallback so the app still runs when storage is blocked. Export writes a JSON backup. Import merges a backup back in, sanitizing every incoming row and skipping anything whose start and end times already exist, so importing the same file twice is harmless and moving history between a phone and a laptop keeps both sets.

An optional short WebAudio tone can announce target completion. It defaults to off.

## Stack

One `index.html` file. Vanilla HTML, CSS, and JavaScript, inline, no build step and no frameworks. The ring is hand drawn SVG with arc paths computed from polar coordinates, not canvas and not a charting library. Google Fonts (Space Grotesk) is the only external resource. No network calls, no accounts, no analytics. Everything stays in your browser.

## Safety

Informational tracking only. This is not medical advice. Talk with a clinician before changing how you eat, especially if you are pregnant, managing diabetes, or taking medication.

---

*Ship something every day. It compounds.*
