IMPACT LAB PITCH DISPLAY
========================
Open index.html in Chrome or Edge (double-click). Press F11 for fullscreen
on the projector. Everything is local — no wifi needed.

THE FOUR SCREENS (number keys)
  1  Pre-show   walk-in screen: title, the three problems, "sign-ups close 6:40"
  2  Roster     full running order in three theme columns
  3  Pitches    the live screen: presenter + 60-second countdown video
  4  Break      team formation menu over the dancing-crab loop

KEYS DURING PITCHES
  Space  start / pause the countdown
  ->     current pitch done, next one up
  <-     back one
  S      skip (they re-queue at the end of their theme)
  R      reset the countdown for this pitch
  M      sound on / off
  ?      key cheat-sheet on screen

ADMIN (press A)
  Entry flow at 6:40: pick the theme to match the paper sheet, then for each
  row: Name, Tab, one-line summary, Enter. Repeat per sheet. ~24 rows takes
  8-10 minutes. Edits mid-show are fine — the screen updates instantly.
  E exports the roster as CSV (this is the Slack/team-formation record).

ORDER OF OPERATIONS ON THE NIGHT
  5:45  open index.html, F11, press 1. Sound check: M toggles.
  6:30  press A, type sheet contents so far (sheets walked over at 6:30)
  6:50  sign-ups close; type the rest, close drawer (Esc)
  6:55  press 2 (roster up through the 7:00 kickoff)
  7:15  press 3, Space when the first pitcher is ready
  7:50  press 4 (break + team formation), press E for the CSV
  8:50  close block; VENUE HARD STOP 9:00 - everyone out
  If the room runs long on applause: -> is always safe; the video resets
  per pitch automatically.

IF SOMETHING BREAKS
  - Browser crash: reopen index.html. It restores the same pitch and
    remaining time.
  - Countdown video won't play: admin (A) -> untick "use countdown video".
    A rendered 60s timer takes over. The show goes on.
  - Wrong name on screen: A, fix it in the table, Esc.
  - Nuclear option: admin -> Reset everything (wipes the roster).

FILES (move this whole folder — it is self-contained)
  index.html        the whole app
  claude-logo.png   the Claude starburst (roster + break headers)
  intro.mp4         11.7s Anthropic sting (plays entering pitches + each new theme)
  countdown-60.mp4  the 60-second timer video
  dance-loop.mp4    break-screen loop (its music is loud — M mutes)
  horn-1.wav        time's-up horn (car klaxon)
  horn-2.wav        time's-up horn (ship horn) — the two alternate pitch to pitch;
                    M mutes them along with everything else

FIRST RUN ON A NEW LAPTOP
  Double-click index.html, press 1-2-3-4 once to touch every screen, then
  Space on screen 3 to confirm the countdown plays. 30 seconds, do it before
  you leave for the venue.
