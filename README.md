# NAM Tone Matcher for REAPER

A single-file web app that identifies the amp and signal chain used on a recording, searches [TONE3000](https://www.tone3000.com) for the best matching Neural Amp Modeler (NAM) captures and cabinet IRs, and exports a ready-to-use REAPER `.RTrackTemplate` file.

**500+ songs** across Classic Rock, Hard Rock, Metal, Grunge, Alternative, Blues, Prog, Punk, J-Rock, J-Metal, and more — each with documented amp assignments, confidence scores, and source citations.

---

## What You Need

- A modern web browser (Chrome, Firefox, Safari, Edge)
- Python 3 (to run a local HTTP server) — already installed on macOS/Linux; [download for Windows](https://www.python.org/downloads/)
- A free [TONE3000](https://www.tone3000.com) account
- [REAPER](https://www.reaper.fm) DAW
- [Neural Amp Modeler VST3](https://github.com/sdatkinson/NeuralAmpModelerPlugin/releases) plugin by Steven Atkinson

---

## Setup

### 1. Clone or download the repo

```bash
git clone https://github.com/your-username/nam-tone-matcher.git
cd nam-tone-matcher
```

Or just download `nam-tone-matcher.html` directly.

### 2. Start a local HTTP server

The app uses OAuth authentication, which requires serving the file over HTTP — **do not open it by double-clicking** (the `file://` protocol will break the login flow).

Open a terminal in the folder containing `nam-tone-matcher.html` and run:

```bash
python3 -m http.server 8080
```

Then open your browser and go to:

```
http://localhost:8080/nam-tone-matcher.html
```

> **Windows users:** use `python -m http.server 8080` if `python3` is not recognised.

---

## Connecting to TONE3000

TONE3000 provides real NAM captures and cabinet IRs sorted by download count. The app uses OAuth 2.0 with PKCE — you never enter your password into the app.

### Steps

1. Open the app at `http://localhost:8080/nam-tone-matcher.html`
2. Click **Connect TONE3000** in the top-right
3. You will be redirected to TONE3000's login page — sign in (or create a free account)
4. TONE3000 will redirect you back to the app automatically
5. The status chip in the top-right should now show **Connected to TONE3000**

> Your session token is stored in `sessionStorage` and will expire when you close the browser tab. You will need to reconnect each session.

### Token expired?

If searches return no results or you see an auth error, click **Disconnect** and then **Connect TONE3000** again to get a fresh token.

---

## Using the App

### Search by song

Type a song name, artist name, or both into the search bar and press **Enter** or click **Match Tone**.

Examples:
- `master of puppets`
- `metallica master of puppets`
- `slow dancing burning room`
- `what's up people maximum the hormone`

### Browse the full list

Click **Browse ▾** to open a searchable dropdown of all 500+ tracks grouped by genre.

### Reading the results

**Signal Chain card** — shows the identified amp, amp type, gain level, and effects pedals used on the recording. Each result includes:
- **Match confidence** (0–100) — how well-documented the amp assignment is
- **Source** — the specific interviews, album notes, or producer documentation that backs the match

**NAM Captures** — top results from TONE3000 sorted by all-time downloads. Click any card to select it for the template.

**Cabinet IRs** — same for cab impulse responses. Click to select.

**REAPER Track Template** — auto-generated `.RTrackTemplate` with the full FX chain pre-configured. Download it and load it directly into REAPER.

---

## Loading the Template in REAPER

### Step 1 — Download assets

Download your chosen `.nam` file and cabinet `.wav` IR from the TONE3000 links. Save them somewhere you'll remember (e.g. `~/Documents/NAM/`).

### Step 2 — Install the NAM plugin

Download and install **Neural Amp Modeler VST3** from:  
[https://github.com/sdatkinson/NeuralAmpModelerPlugin/releases](https://github.com/sdatkinson/NeuralAmpModelerPlugin/releases)

Scan for new plugins in REAPER: `Options → Preferences → Plug-ins → VST → Re-scan`.

### Step 3 — Import the template

In REAPER:
```
File → Track Templates → Load Track Template…
```
Select the downloaded `.RTrackTemplate` file. A new track will appear with the full FX chain.

### Step 4 — Load the .nam model

Open the **Neural Amp Modeler** plugin on the track. Click the folder icon and navigate to your downloaded `.nam` file.

### Step 5 — Load the cabinet IR

Open the **ReaVerb** instance on the same track. Remove any default impulse, click **Add**, choose **File**, and select your downloaded `.wav` IR.

### Step 6 — Dial in

The template defaults are a starting point. Adjust the ReaEQ, gain staging, and time-based effects to match your recording context.

---

## File Structure

```
nam-tone-matcher.html   # The entire app — HTML, CSS, and JS in one file
README.md               # This file
```

The app is intentionally a single file with no dependencies, no build step, and no server-side code. Everything runs in the browser.

---

## How the Song → Amp Matching Works

The app contains a hand-curated database of 500+ songs. Each entry stores:

- Artist and song name
- Genre
- Amp model, search terms for TONE3000, and cabinet info
- Full pedal chain with corresponding REAPER plugin mappings
- A **confidence score** (0–100) reflecting how well-documented the gear is
- A **source** string describing the documentation (interviews, producer notes, album liner notes)

When you search, the app uses a word-set matching algorithm that:
1. Strips punctuation and common stop words from your query
2. Tries to match a meaningful artist word **and** a meaningful song word simultaneously
3. Falls back to artist-only matching if no song match is found
4. Returns `null` (no match) rather than a false positive if nothing meaningful matches

The TONE3000 search is then run live using your authenticated token, querying the API for NAM captures (`gears=amp&platform=nam`) and IRs (`gears=ir`) sorted by `downloads-all-time`.

---

## Technical Notes

- **Auth:** OAuth 2.0 Authorization Code flow with PKCE. No client secret is used — the public client ID is embedded in the file.
- **API:** TONE3000 REST API v1 (`https://www.tone3000.com/api/v1`)
- **Redirect URI:** Detected automatically from `window.location`. Falls back to `http://localhost:8080/nam-tone-matcher.html` if opened as a `file://` URL.
- **Token storage:** `sessionStorage` only — never written to disk or `localStorage`.
- **Template format:** Plain-text REAPER `.RTrackTemplate` — human-readable and version-control friendly.

---

## Contributing

Pull requests welcome. To add songs to the database, find the `TONE_DB` array in the `<script>` section and add entries following the existing pattern:

```js
{
  artist: "Artist Name",
  song: "Song Title",
  genre: "Genre",
  amp: "Full Amp Model Name",
  ampSearch: "search terms for tone3000",
  cabSearch: "cabinet search terms",
  ampType: "Short amp category",
  gain: "clean | low | medium | high",
  cab: "Cabinet description",
  pedals: [
    { name: "Pedal Name", type: "overdrive", reaperPlugin: "JSFX: overdrive" }
  ],
  confidence: 85,
  source: "Specific documentation source for this amp assignment."
}
```

---

## Credits

- NAM captures and cabinet IRs served by [TONE3000](https://www.tone3000.com)
- Neural Amp Modeler plugin by [Steven Atkinson](https://github.com/sdatkinson/NeuralAmpModelerPlugin)
- REAPER DAW by [Cockos](https://www.reaper.fm)
