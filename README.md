# Impact Lab Pitch Display

Single-file event app that ran the pitch block at the **Claude Community Atlanta Impact Lab** (August 12, 2026): who's pitching, what they're building, which problem theme, a 60-second countdown, and who's next. Driven entirely by keyboard from the MC position.

## Run it

Double-click `index.html` (Chrome or Edge), press **F11** for fullscreen. Everything is local — no network, no build, no install. The whole folder is the deployment; move it as a unit.

## Screens & keys

| Key | Screen |
|---|---|
| `1` | Pre-show walk-in screen |
| `2` | Roster (three theme columns) |
| `3` | Live pitches — presenter + 60-second countdown video |
| `4` | Break / team-formation menu |

During pitches: `Space` start/pause, `→` next, `←` back, `S` skip (re-queues at end of theme), `R` reset, `A` admin drawer, `M` mute, `E` export roster CSV, `?` on-screen cheat sheet.

State persists to localStorage on every change — a browser crash mid-timer restores the same pitch with the same remaining time.

## Docs

- [`README.txt`](README.txt) — the operator manual (night-of run order, entry flow, failure playbook)
- [`CLAUDE.md`](CLAUDE.md) — dev notes: architecture constraints, fixed bugs not to reintroduce, verification recipe

## Media

The `.mp4` files are stored in **Git LFS** (the countdown video alone is 153 MB — clone with `git lfs` installed). 
