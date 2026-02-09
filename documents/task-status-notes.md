# Task Status Notes

Use this file to append notes at the end of each completed task. Keep entries concise and actionable so work can resume after context is cleared.

## Entry Template
- Date:
- Task completed:
- Files touched:
- Decisions / rationale:
- Follow-ups / next steps:
- Open questions / risks:
- Testing / verification:

## Notes

- Date: 2026-02-08
- Task completed: Initial app implementation (UI shell, detection loop, recording + IndexedDB, review timeline, PWA scaffolding, GitHub Pages workflow).
- Files touched: /Users/mdn/Projects/deadhang-monitor/index.html, /Users/mdn/Projects/deadhang-monitor/manifest.json, /Users/mdn/Projects/deadhang-monitor/sw.js, /Users/mdn/Projects/deadhang-monitor/robots.txt, /Users/mdn/Projects/deadhang-monitor/.nojekyll, /Users/mdn/Projects/deadhang-monitor/app-icon.svg, /Users/mdn/Projects/deadhang-monitor/favicon.svg, /Users/mdn/Projects/deadhang-monitor/.github/workflows/deploy.yml
- Decisions / rationale: Single-page HTML with MoveNet via CDN; 10 fps sampling; IndexedDB single store for sessions (frames embedded) for simplicity; dead-hang heuristic = arms straight + wrists above shoulders + feet off inferred via knee bend or ankle lift; review UI uses scrub slider + set start/end buttons; deploy version injection placeholder in index.html.
- Follow-ups / next steps: Manually test camera flow, detection stability, and review adjustment; validate IndexedDB persistence; refine thresholds; consider frame size impacts on storage.
- Open questions / risks: Feet-off-ground heuristic is approximate; confirm in real usage; consider adding low-confidence handling on knees/ankles.
- Testing / verification: Not run yet.

- Date: 2026-02-08
- Task completed: Added tuning controls and additional live metrics to support threshold testing; tightened hang state handling when detection drops.
- Files touched: /Users/mdn/Projects/deadhang-monitor/index.html
- Decisions / rationale: Added UI sliders for confidence/angles/feet lift/overhead margin + camera facing mode; persisted settings in localStorage for repeat testing; show elbow/knee angles for easier calibration; ensure hang state resets on frames with no pose.
- Follow-ups / next steps: Run local server and manually test thresholds with real hangs (desktop + mobile). Adjust defaults as needed.
- Open questions / risks: Camera facing mode switch requires restart; real-world feet-off-ground heuristic still needs validation.
- Testing / verification: Not run yet (requires webcam).

- Date: 2026-02-08
- Task completed: Added sound feedback toggle + tone gating; added timeline markers for start/end on review slider.
- Files touched: /Users/mdn/Projects/deadhang-monitor/index.html
- Decisions / rationale: Sound uses Web Audio oscillator (432 Hz) with gain ramp; tone active when not hanging and stops during hang; toggle stored in settings; timeline markers show start/end positions for review.
- Follow-ups / next steps: Manually verify audio behavior on desktop/mobile (auto-play restrictions); validate marker positions after adjustments.
- Open questions / risks: AudioContext resume may require user gesture if toggled during detection; adjust tone volume if too loud.
- Testing / verification: Not run yet.

- Date: 2026-02-08
- Task completed: Header switches to large timer during hang; sound gating uses debounced hang state; added timeline start/end markers.
- Files touched: /Users/mdn/Projects/deadhang-monitor/index.html
- Decisions / rationale: Header timer persists once a hang is detected until camera stop; timer displays mm:ss; sound toggles only when debounced hang state changes to avoid jitter.
- Follow-ups / next steps: Verify header swap on first hang; confirm tone stays off during slight movement; validate marker positions on review slider.
- Open questions / risks: None beyond audio autoplay quirks.
- Testing / verification: Not run yet.

- Date: 2026-02-08
- Task completed: Hide Live Session panel until camera starts; enlarge header timer and reduce padding; move history list below review controls.
- Files touched: /Users/mdn/Projects/deadhang-monitor/index.html
- Decisions / rationale: Header timer uses timer-active class for large text and tighter padding; Live Session panel hidden by default and shown only while detecting; history list moved below review/timeline controls per UX request.
- Follow-ups / next steps: Verify header timer size on mobile; ensure history list layout feels balanced with review panel.
- Open questions / risks: None.
- Testing / verification: Not run yet.
