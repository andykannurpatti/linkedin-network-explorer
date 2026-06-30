# LinkedIn Network Explorer

Turn your LinkedIn connections export into a searchable, filterable network — segmented by industry, function, seniority, and (optionally) relationship strength. Includes an AI-powered Project Matcher that suggests the most relevant people for a project you describe, using either a local Ollama model or the Claude API.

Built by [Andy Kannurpatti](https://www.mandalasustainablesolutions.com/principal) · [Mandala Sustainable Solutions](https://www.mandalasustainablesolutions.com)

---

## Quick start

1. **Download [`index.html`](./index.html)** from this repo (click the file, then the "Download raw file" button — or `git clone` the repo).
2. **Double-click it.** It opens directly in your browser. That's it — no installation, no terminal, no build step.

You do **not** need VS Code, Node.js, npm, Live Server, or any developer tooling. This is a single self-contained HTML file.

> The first time you open it, you'll need an internet connection — it loads React and Babel from a CDN. After that, all of your data processing happens entirely offline in your browser.

---

## Getting your LinkedIn export

1. Go to LinkedIn → **Settings & Privacy → Data privacy → Get a copy of your data**
2. Select **"Connections"** (required). Optionally also select **"Messages"** if you want relationship-strength scoring.
3. LinkedIn will email you a download link once the export is ready (usually within a few minutes to an hour).
4. Unzip the download — you'll get `Connections.csv` and, if requested, `messages.csv`.

---

## Using the tool

1. Open `index.html`.
2. Drag and drop (or click to browse) your `Connections.csv` file. `Messages.csv` is optional — upload it if you want a "Relationship Strength" filter based on your message history.
3. Click **Build my network explorer**.
4. Filter by Industry, Function, Seniority, Connection Age, Status, and (if you uploaded Messages.csv) Relationship Strength. Use the search bar for free-text search by name, company, or role.
5. Each connection card links directly to their LinkedIn profile.

### Customizing categories

Click **"Customize Function categories"** to add your own keyword-based categories on top of the built-in ones (e.g. add "Maintenance & Reliability" with keywords `maintenance, reliability, turnaround`). Custom rules take priority over the built-in classifier and are scoped to your current session only.

### AI Project Matcher

Describe a project or activity (e.g. *"I'm raising a seed round for a climate tech startup and need warm intros to industrial VCs"*), and the tool will suggest the most relevant people in your network, with a short reason for each match.

Two options for the AI backend:

- **Ollama (local)** — point it at a locally running Ollama instance (default `http://localhost:11434`) and a model you've already pulled (e.g. `llama3.1`). Nothing leaves your machine. Best for smaller filtered pools — local models can be slow on large datasets, so narrow with the filters first.
- **Claude API** — paste your own Anthropic API key (stored only in browser memory for that tab session, never written to disk or sent anywhere except `api.anthropic.com`). Handles large datasets comfortably.

**Tip:** Apply filters *before* using the Project Matcher — it only searches the connections currently visible after your filters, so narrowing first (e.g. to one Industry or Function) gives faster, more focused results, especially with Ollama.

---

## Privacy & data handling

- **Nothing is uploaded anywhere.** All CSV parsing, classification, and filtering happens locally in your browser using JavaScript's `FileReader` API. There is no backend server.
- **No data is cached or persisted.** Refreshing the page or closing the tab clears everything — you'll need to re-upload your CSV(s) next time.
- **The Project Matcher is the one exception.** If you use it, a compact summary of your *currently filtered* connections (name, title, company, industry, function, seniority, relationship strength — not raw CSV data) is sent to whichever backend you chose:
  - **Ollama**: sent only to the local address you specify — typically your own machine.
  - **Claude API**: sent to Anthropic's API (`api.anthropic.com`) for that single request. Review [Anthropic's usage policies](https://www.anthropic.com/legal) before enabling this if you have data sensitivity concerns.

---

## Intended use & limitations

- This tool is intended for **personal networking and research use** — segmenting and searching your own LinkedIn connections.
- Industry, Function, Seniority, and Connection Status are **inferred automatically** from job titles and company names using keyword matching and a curated company lookup table. These classifications are estimates and **can be wrong or incomplete** — verify before relying on them for outreach, hiring, fundraising, or other decisions.
- Relationship Strength (if enabled via Messages.csv) is a heuristic based on message count, recency, and whether the conversation was two-way — it is **not** a measure of actual relationship quality.
- **This tool is not affiliated with, endorsed by, or connected to LinkedIn Corporation in any way.**
- Only use this tool with data you are authorized to access — i.e., your own LinkedIn data export. Use of LinkedIn data remains subject to [LinkedIn's Terms of Service](https://www.linkedin.com/legal/user-agreement). Do not use this tool to facilitate bulk scraping, spam, or unsolicited outreach.
- Provided as-is, with no warranty. Use at your own discretion.

---

## Technical details

- Single-file React app (loaded via CDN, compiled in-browser with Babel — no build step or bundler required).
- CSV parsing is a minimal custom parser handling LinkedIn's export format, including quoted fields and the "Notes:" preamble LinkedIn prepends to `Connections.csv`.
- Company-to-industry classification uses a lookup table of ~150 well-known companies (chemicals, semiconductors, software, biotech/pharma, finance, VC, consulting, etc.), falling back to keyword matching on job title and company name for everything else.
- No external dependencies beyond React, ReactDOM, and Babel Standalone (loaded from unpkg.com).

### Want to modify it?

The entire app is in the single `index.html` file — open it in any text editor. The React/JSX source lives inside the `<script type="text/babel">` tag near the bottom of the file. There's no separate build process; edit the JSX directly and reload the page in your browser to see changes.

---

## License

Provided as-is for personal and research use. See [Intended use & limitations](#intended-use--limitations) above.
