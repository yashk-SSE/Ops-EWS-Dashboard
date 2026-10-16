# Ops EWS Dashboard

Live web view of SolarSquare's Ops early warning sheet (EWS): customers between installation and commissioning who
write to us on Freshchat. It answers "what to do today, and where": customers to call, projects to expedite, chats waiting
for a reply, harsh words - India, by cluster and by cell.

Live at **https://yashk-sse.github.io/Ops-EWS-Dashboard/** (GitHub Pages, from `main`).

## What is on it

- **What to do today** - the "Do first" line, four action cards (Call now, Expedite stuck projects, Reply on Freshchat,
  Harsh words) with who acts, and the next level down worst first: States, then clusters, then cells.
- **States & clusters** - the India summary table, grouped by State. State comes from Dilip's FC_Data (City -> State).
- **Pending chats** - who wrote to us and still waits for an agent reply, by age; EWS sheet (customers) next to the
  Commissioning Dashboard (chats), and why the two differ.
- **Customer list** - Project IDs grouped by what to do (call, expedite, waiting for a reply, not in Freshchat), with
  filters (reason, waiting time, cell, search) and a CSV download.
- **Trend** - one point a day from the sheet's History tab; message types of open rows.

## Data

`data/ews.json` is written by the Ops sheet's Apps Script (`Ops-Flag-Sheet-AppsScript/Code.gs`, functions
`buildEwsJson_` and `pushDashboard_`) through the GitHub API after every pull (09:30, 14:30) and every
Commissioning Dashboard run. Nothing else writes it. The token sits in the script's Script Properties
(`GITHUB_TOKEN`, fine-grained, this repo only, Contents read and write).

Names in the data stay as they were before "push" became "expedite" (16 Oct, script v6.27): Actions `Call + push` and
`Project stuck - push it`, action columns `Call + push` and `Push stuck`, link key `push`. The page shows the new words
(`actLabel` in `index.html`) and also accepts data sent in the new words.

**This is a public page: counts and Project IDs only.** No customer names, phones, message text or the harsh word
matched. Those stay in the Google Sheet, which only the Ops team can open (the "Open in the sheet" buttons need a
SolarSquare login with access).

## Files

- `index.html` - the whole page (no build step). Chart.js 4.4.1 from cdnjs, DM Sans / DM Mono from Google Fonts.
- `logo.jpg` - SolarSquare logo (same as the Referral Dashboard).
- `data/ews.json` - written by the Apps Script (see above).

Design (palette, type, top bar, sidebar, cards, tables) follows the Referral Dashboard
(`yashk-SSE/Referral-Dashboard`).

## Local preview

The script commits `data/ews.json` after every pull, so run `git pull --rebase` before pushing page changes.


Serve the folder (`python -m http.server 8771`) and open `http://localhost:8771/`. For real-sized data, build a
local `data/ews.json` from a Drive export of the sheet with `tools/ops_flags/ews_json_from_export.py` (in the
project workspace) - for preview only, never commit it; the live file comes from the Apps Script.
