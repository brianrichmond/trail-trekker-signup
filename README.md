# Sheet Scan Tool
Converts paper sign-up sheet photos or PDFs into a CSV for import into a mail handling platform or other database. Built initially for the El Cerrito Trail Trekkers and El Cerrito Fire Safe Council.


**Live site:** `https://brianrichmond.github.io/sheet-scan/`

---

## How it works

- `index.html` — user-facing welcome page with setup instructions; served by default at the root URL
- `signup-to-csv.html` — the app itself; uses PDF.js to render PDFs and the Claude API to extract handwritten data
- A Cloudflare Worker (`gentle-lake-2a35.brian-richmond.workers.dev`) acts as a CORS proxy, forwarding requests from the browser to the Anthropic API

Users supply their own Anthropic API key, stored in browser localStorage. No data is stored by this app.

---

## Output columns

| Column | Notes |
|---|---|
| Email Address | |
| First Name | |
| Last Name | |
| Address | |
| Phone | |
| Signup Date | From event header |
| Signup Event | From event header |
| Event Source Raw | Verbatim "How did you hear?" response |
| Event Source | Normalized (word of mouth / El Cerrito website / social media / email / flyer or poster / organizer / other) |

---

## Updating the app

Push changes to `main` — GitHub Pages deploys automatically within ~60 seconds.

To update the Cloudflare Worker, log in at [dash.cloudflare.com](https://dash.cloudflare.com) → Workers & Pages → edit and redeploy.

---

## Architecture notes

- PDF rendering: [PDF.js 3.11](https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js) via cdnjs
- AI extraction: Claude Haiku via Anthropic API
- CORS proxy: Cloudflare Worker (required because browsers block direct calls to api.anthropic.com)
- Hosting: GitHub Pages (static, no backend)

---

## Known issues / tips for maintainers

- iPhone photos taken in HEIC format will fail — users should set Camera → Formats → Most Compatible, or use Share → Save to Files to convert
- Images are resized to max 2000px before sending to avoid API payload limits
- If the Cloudflare Worker goes down, all users will see "Failed to fetch" errors
