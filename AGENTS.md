# AGENTS.md

Instructions for AI coding agents working on this project.

## Project Overview

DefconCam is a Raspberry Pi camera surveillance system with missile alert monitoring. It runs on a Pi (tomer@10.0.0.238, hostname "pi-hole") with a Logitech BRIO webcam, streaming via mjpg-streamer + ffmpeg with OSD overlays.

## Architecture

    mjpg-ctl (control script)
      ├── generates → mjpg-rotated.sh (auto-generated launcher, gitignored)
      ├── calls → v4l2-ctl (camera hardware controls)
      └── restarts → mjpg-streamer.service
    mjpg-alert (Python, alert monitor + Twitter bot)
      ├── polls → Pikud HaOref API
      ├── writes → OSD text files (/tmp/mjpg-*.txt)
      └── calls → tweepy (Twitter API)
    mjpg-auto (Python, cron every 15min)
      └── calls → mjpg-ctl day/night
    mjpg-web (Python HTTP server, port 8081)
      └── calls → mjpg-ctl via subprocess

All scripts in `bin/` are symlinked to `/usr/local/bin/`. Do not edit files in `/usr/local/bin/` directly — edit the repo source.

## Critical Rules

### Tweeting Policy
- Only tweet on DEFCON 2 (incoming missiles). One tweet per alert.
- Never tweet on ALL CLEAR or DEFCON 5.
- Wait 3 seconds after updating OSD text before taking a snapshot, so ffmpeg has time to render the new overlay.

### State Machine
- States: idle (DEFCON 5) → alert (DEFCON 2) → clear (ALL CLEAR) → idle
- Only transition alert → clear on explicit "האירוע הסתיים" from the API. Empty API response does NOT clear the alert.
- State is persisted to `/tmp/mjpg-alert-state`. Always read it on startup to survive restarts.
- Never reset to DEFCON 5 just because the service restarted.

### OSD Text Files
- Files: `/tmp/mjpg-idle.txt`, `/tmp/mjpg-alert.txt`, `/tmp/mjpg-clear.txt`, `/tmp/mjpg-osd.txt`
- These MUST exist before ffmpeg starts or it will crash.
- Write a space `" "` to clear a file, never write empty string `""` — ffmpeg's textfile reload ignores empty files.
- `mjpg-rotated.sh` creates these on startup as a safety net.
- `mjpg-alert` also creates them on startup.

### ffmpeg Filter Gotchas
- Never use colons `:` in drawtext text values — they break ffmpeg's filter parser. Use dots instead (e.g., `16.54` not `16:54`).
- Use `textfile` with `reload=1` for dynamic content. Static `text=` values are baked at launch.
- Always use `-atomic_writing 1` to prevent race conditions between ffmpeg writing and mjpg-streamer reading.
- The OSD settings line uses `textfile=/tmp/mjpg-osd.txt:reload=1` so brightness updates show live without restarting the stream.

### mjpg-rotated.sh is Auto-Generated
- `mjpg-ctl`'s `apply()` function generates `bin/mjpg-rotated.sh`. Do not hand-edit it.
- It is gitignored. Any fix must go into `mjpg-ctl`'s `apply()` function or it will be overwritten.
- Changes to the ffmpeg filter chain, v4l2 settings, or OSD layout must be made in `mjpg-ctl`.

### Service Ordering
- `mjpg-alert.service` must start before `mjpg-streamer.service` (configured via `After=` in systemd unit).
- This ensures OSD text files exist before ffmpeg needs them.

### Camera Hardware (Logitech BRIO)
- Device: `/dev/video0`
- Day mode: `auto_exposure=1` (manual), `exposure_time_absolute=3` (minimum), `gain=0`, `backlight_compensation=1`, `contrast=80`
- Night mode: `auto_exposure=3` (auto), `gain=255`
- Indoor mode: `auto_exposure=3` (auto), `gain=128`
- Focus: `focus_automatic_continuous=1` (auto). Refocus by toggling off/on with 1s delay.
- Rotation: only 0/90/180/270 via ffmpeg transpose. No arbitrary angles.
- Sweet spot: 720p@24fps. 1080p@30fps overloads the Pi CPU (~240%).
- USB disconnects happen (especially with long/cheap cables) — ffmpeg dies but mjpg_streamer stays running. Service restart fixes it.
- `KillMode=mixed` and `TimeoutStopSec=5` in systemd service to handle stuck ffmpeg processes.

### Brightness Control
- `mjpg-ctl bright <0-100>` adjusts brightness live without restarting the stream.
- It writes to `/tmp/mjpg-osd.txt` so the OSD updates dynamically.
- Other settings (mode, resolution, rotation, fps) require a full `apply()` which restarts the stream.

## Secrets
- Twitter API keys are in `/etc/mjpg-twitter.conf` (not in repo, gitignored).
- Format: `API_KEY="..."`, `API_SECRET="..."`, `ACCESS_TOKEN="..."`, `ACCESS_SECRET="..."`
- Pre-commit hook runs gitleaks to prevent accidental secret commits.
- Never hardcode credentials in scripts. Always read from the config file at runtime.

## SSH / Remote Editing
- When writing scripts via SSH heredocs, beware of variable expansion and special character escaping.
- Prefer piping file content via `cat file | ssh host "cat > path"` over complex heredocs with backticks, dollar signs, or backslashes.
- Always run `python3 -m py_compile <script>` after editing Python files to catch syntax errors before restarting services.
- After editing bash scripts, verify with `bash -n <script>`.

## File Locations

### In Repo (~/defcon-cam/)
- `bin/mjpg-ctl` — control script
- `bin/mjpg-alert` — alert monitor + Twitter bot
- `bin/mjpg-auto` — auto day/night switcher
- `bin/mjpg-web` — web control panel
- `config/mjpg-streamer.conf` — persisted camera settings
- `systemd/*.service` — systemd unit files

### External (not in repo)
- `/etc/mjpg-twitter.conf` — Twitter API credentials
- `/etc/mjpg-next-switch` — next day/night switch time (chmod 666)
- `/tmp/mjpg-alert-state` — persisted DEFCON state
- `/tmp/mjpg-osd.txt` — dynamic OSD settings text
- `/tmp/mjpg-idle.txt` — DEFCON 5 overlay text
- `/tmp/mjpg-alert.txt` — DEFCON 2 overlay text
- `/tmp/mjpg-clear.txt` — ALL CLEAR overlay text
- `/tmp/mjpg-auto.log` — cron log

## Alert Monitoring
- API: https://www.oref.org.il/warningMessages/alert/Alerts.json
- Fuzzy matching: watch terms are substrings matched against city names in the data array
- Current watch terms: גבעתיים, רמת גן, תל אביב, בני ברק
- Poll interval: 3 seconds
- ALL CLEAR display timeout: 300 seconds before returning to DEFCON 5
