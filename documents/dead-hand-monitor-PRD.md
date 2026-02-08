# Deadhang Monitor PRD

## 1) Summary
A browser-based Deadhang Monitor that uses webcam pose estimation to detect dead hangs, track duration, and let users correct start/stop by scrubbing recorded pose data. Fully client-side, privacy-first, PWA-ready, and deployable to GitHub Pages.

## 2) Goals
- Detect dead hangs in real time and show a simple duration timer.
- Record pose data for each session so users can review and adjust start/stop.
- Work well on both desktop and mobile.

## 3) Non-Goals (MVP)
- No accounts, cloud sync, or backend.
- No “active vs passive hang” detection.
- No rep/set tracking or form scoring beyond “dead hang state.”

## 4) Definition of “Dead Hang”
- Arms straight overhead.
- Feet off the ground.
- Knees can be bent (knee-bend may indicate feet off ground).

## 5) Core Functional Requirements
1) Live Detection
- Start/Stop/Reset session.
- Live duration timer (seconds).
- Status: “Ready / Hanging / Not detected / Invalid.”

2) Pose Data Recording
- Store per-frame pose keypoints + confidence + timestamp for the session.
- Save after Stop as part of the history item.
- Sampling rate: 10 fps.
- Storage target: IndexedDB.

3) Timeline Review & Adjustment
- Timeline scrubber (slider) that replays stored poses.
- User can adjust start and stop markers if auto-detection is off.
- Adjusted times update final duration.

4) History
- History list of past hangs with duration.
- Each history item includes recorded pose data + start/stop markers.
- No charts needed; time-centric review only.

5) Persistence
- Store history locally (IndexedDB).
- Delete single + delete all.

6) PWA
- Installable; minimal service worker (no aggressive caching).

7) Deployment (GitHub Pages)
- GitHub Actions workflow like `squat-meter`:
  - Trigger on `push` and `pull_request` to `main`.
  - Configure Pages.
  - Inject `__DEPLOY_VERSION__` into `index.html` using a UTC timestamp.
  - Upload and deploy via `actions/deploy-pages@v4`.

## 6) Detection Logic (Initial Approach)
- Use MoveNet single-pose.
- Heuristics:
  - Arms straight: elbow angle near 180° (threshold TBD).
  - Arms overhead: wrist/hand y-position above shoulder.
  - Feet off ground: infer via knee-bend or ankle height relative to hip/ground plane.
- Low confidence → “Not detected,” no state transitions.

## 7) UX / UI Direction
- Palette: Golden taupe
  - #D4AF37
  - #BDB76B
  - #FDFBD4
  - #CE8946
- Layout:
  - Header + subtitle.
  - Viewer (video/canvas).
  - Live stats overlay (Duration + Status).
  - Start/Stop/Reset controls.
  - Timeline scrubber + start/end handles for review.
  - History list with duration; click to review.

## 8) Data Model (History Item)
- id
- startedAt (timestamp)
- durationMs
- startFrameIndex / endFrameIndex (adjustable)
- frames: array of { t, keypoints[], score }

## 9) Risks & Mitigations
- Pose confidence on mobile / low light → “Not detected.”
- Storage size for pose data → 10 fps sampling + IndexedDB.
- Feet-off-ground inference is tricky → allow user correction via timeline.

## 10) Implementation Plan (Checklist)
### Foundation
- [ ] Define app shell: basic HTML layout, viewer container, controls, stats overlay.
- [ ] Establish Golden taupe palette + base typography and spacing.
- [ ] Add PWA scaffolding: manifest, icons, minimal service worker.

### Pose Detection & Live Session
- [ ] Integrate TF.js + MoveNet (SINGLEPOSE_LIGHTNING) via CDN.
- [ ] Implement camera access + start/stop handling.
- [ ] Render video to canvas and draw pose overlay.
- [ ] Compute keypoint confidence and guard low-quality frames.
- [ ] Implement dead-hang detection state machine (arms overhead + elbows straight + feet off ground heuristics).
- [ ] Live timer + status updates.
- [ ] Reset behavior (clears live session only).

### Recording & Persistence
- [ ] Sample pose frames at 10 fps and store in memory during session.
- [ ] Define IndexedDB schema (sessions + frames).
- [ ] Persist finished session to IndexedDB with start/end indices and duration.
- [ ] Load sessions on startup and render history list.

### Review & Adjustment
- [ ] Build timeline UI (slider) for session review.
- [ ] Render pose for a scrubbed frame.
- [ ] Add adjustable start/end markers.
- [ ] Recompute duration when markers change.
- [ ] Update stored session on save/exit.

### History Management
- [ ] Select session from list to review.
- [ ] Delete single session.
- [ ] Delete all sessions.
- [ ] Optional: undo delete (if time).

### Deployment
- [ ] Add GitHub Pages workflow with version injection (`__DEPLOY_VERSION__`).
- [ ] Ensure index.html includes deploy version tag.

### QA / Testing
- [ ] Verify desktop and mobile camera permissions.
- [ ] Verify hang detection with knees bent and feet off ground.
- [ ] Validate timeline adjustments affect duration correctly.
- [ ] Confirm IndexedDB persistence across reloads.
- [ ] Confirm GitHub Pages deploy works on main branch.
