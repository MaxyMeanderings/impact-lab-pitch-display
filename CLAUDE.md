# Impact Lab Pitch Display

Single-file event app for the **Claude Community Atlanta Impact Lab, Wednesday 12 August 2026, 6:00–9:30pm**. It runs the pitch block on the venue projector: who's pitching, what they're building, which problem, a 60-second countdown, who's next. Tyler (host) drives it by keyboard while MCing. `README.txt` is the operator manual — keep it accurate if you change behavior.

**This folder is the whole deployment.** `index.html` + media, opened via double-click (`file://`), fullscreened with F11. No network, no build, no install. Every change must keep it working from a cold double-click on a machine with no wifi.

## Event context (why things are the way they are)

- ~60–80 attendees expected. Max 24 pitch slots on three paper sign-up sheets at the door (8 per problem theme). Sign-ups close 6:40; a floater types them into the admin drawer 6:40–6:53; pitches run 6:55–7:40.
- **Pitch length is exactly 60 seconds and is not configurable in practice** — the timer is Anthropic's produced `countdown-60.mp4`, and the attendee email promised "one minute." Don't add other durations to the primary path. The rendered fallback timer exists only for video failure.
- Run order is theme-by-theme (Critical Thinking → Guardrails for Kids → Access), matching the three walls people walk to for team formation at 7:40. The three problem statements baked into the app were reconstructed from Night One (3 Aug) attendee cards — treat their wording as locked.
- The CSV export (`E`) is the team-formation record and Slack channel list. Columns: slot, name, summary, theme, status.

## Architecture (deliberate, don't "improve" away)

- **One `index.html`**, vanilla JS, no modules, no fetch, no CDN — `file://` forbids or breaks all three.
- State is one object `S` persisted to localStorage (`kt3-pitch-display-v1`) on every mutation. Crash recovery: reopening restores view, current pitch, and remaining time. `S.videoTime` is snapshotted every second while the countdown runs.
- Screens: `preshow | roster | live | break`, keys 1–4. `live` overlays presenter info on the countdown video; `break` overlays the team menu on `dance-loop.mp4`.
- Keyboard only (stage lights, no mouse): Space, ←/→, S skip, R reset, A admin drawer, M mute, E export, ? help. Key handler ignores keystrokes while typing in inputs.
- Videos play only from real keypresses (autoplay policy needs a user gesture). All `play()` calls have `.catch(()=>{})`.
- Horns: `horn-1.wav` (klaxon) and `horn-2.wav` (ship) alternate on timer expiry, index in `S.hornIdx` so alternation survives reload. Muted expiry does not consume a turn.

## Bugs already fixed — do not reintroduce

1. **Never call `vid.pause()` unconditionally in `renderLive()`** — it re-pauses playback started a tick earlier by `toggleTimer()`.
2. **Seeks are guarded by a >0.75s delta** (`renderLive`'s `seek()`). A redundant seek clamps `currentTime` to 0 on HTTP servers without Range support (`python -m http.server`!). Symptom: pause snaps the countdown back to 60. Over `file://` seeking is fine; the guard keeps both worlds working.
3. **`fitName()` is synchronous, not rAF** — an rAF fit can fire while the screen is `display:none` (zero widths, loop skips) and never re-run. If you add auto-fit text anywhere, measure synchronously and guard `clientWidth === 0`.
4. **Any full-bleed video needs `position:absolute; inset:0; object-fit:cover`** — an in-flow `<video>` pushed the break columns off-screen.

## Verification recipe

- Serve with `python -m http.server` for browser-driven testing, but remember gotcha #2: **seek-to-resume will clamp to 0 over that server**. That is a harness artifact, not an app bug. Judge crash-recovery over `file://`.
- Playwright blocks `file://` navigation; the in-app preview needs the pane visible for screenshots. DOM/JS assertions work regardless.
- For programmatic tests, set `S.config.sound=false` first so muted `play()` is allowed without a gesture; simulate expiry by calling `vid.onended()` directly.
- Acceptance list lives in `KT3-Atlanta-Pitch-Display-Spec.md` in Tyler's aurora-ops folder (desktop machine); the load-bearing ones: 24-row entry under 10 min, kill-and-reopen mid-timer, skip re-queues at theme end, airplane-mode reload, 30-char names, CSV opens in Excel.

## Brand

Palette sampled from the official media: clay `#D97757` (exact color of `claude-logo.png`, the real 12-spoke Claude starburst — not 8), ivory `#F0EEE6`, cream `#FFFDFA`, ink `#0E0E0C`, char `#1F1E1B`. Theme colors: terracotta / sage `#7A9B76` / slate `#8FA3BF`. Type: Georgia/serif display (matches the "Atlanta" lockup), Segoe UI for wide-tracked uppercase labels (`.caps`), Consolas only for slot numbers and the fallback timer. The preshow screen recreates the "CLAUDE COMMUNITY / Atlanta / IMPACT LAB" lockup: arched SVG textPath + grid mark with offset cream square (the hand from the original lockup was deliberately not attempted). If Tyler supplies the real lockup PNG, replace that recreation with the image.

## Sound levels (measured)

`intro.mp4` mean −29 dB, `countdown-60.mp4` mean −31 dB (both quiet), `dance-loop.mp4` mean −24 dB / peaks −5 dB (**loud music**). The PA decision happens at the venue; `M` mutes everything including horns.

## Open items

- `video impact lab outro.mp4` (103 MB) sits in the parent folder, unwired. Candidate: a `5`/close state for the 9:20 wrap.
- Venue test not yet done: play the countdown twice back-to-back on the actual laptop + projector at its real resolution; decide sound on/off on the PA.
- Tested in Chrome/Edge. Tyler's default browser is DuckDuckGo (Chromium-based, should match, unverified in depth). Whichever browser passes the poke-test tonight is the one for the venue — no switching day-of.
- Fresh machine = fresh localStorage: roster will be empty. Admin drawer has "Load demo roster" for rehearsal and "Reset everything" before doors.
