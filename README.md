# H-1B Sponsor Directory

A filterable directory of real H-1B sponsors, built from the USCIS **H-1B Employer Data Hub**
export (`Employer_Information.csv`, one fiscal year).

**Live site: https://shaheen2300.github.io/Job-portal-directory/**

Built for international students and job seekers who want a quick way to find
employers with a real H-1B sponsorship track record, filtered by state, company
size, and field.

- **43,005 unique employers**, deduplicated and rolled up across every worksite/state they filed from
- **Scale**: Small (1–4 approved petitions, 35,696 employers), Mid (5–24, 6,100), Large (25+, 1,209), a proxy for H-1B hiring volume
- **Field**: Construction, Medical, IT, Engineering, Manufacturing, Finance, Trade, Education, or
  Other/General — inferred from each employer's NAICS industry code, refined with name-keyword
  matching where the code is too broad (e.g. splitting "Professional/Scientific/Technical Services"
  into IT vs. Engineering)
- One-click **Careers search** link per company (a Google search for "[company] careers", since
  USCIS doesn't publish employer websites)

No backend, no build step — plain HTML/CSS/JS. `index.html` fetches `data.json` at load time.

## Files

```
Job-portal-directory/
├── index.html      the app (HTML/CSS/JS, no framework)
├── data.json       the dataset (43,005 rows: [name, state, total_petitions, tier, category])
├── package.json    one script: `npm start`
├── .vscode/        recommends the Live Server extension, pre-set to port 5500
└── README.md
```

## Deployment

The site is served by **GitHub Pages** straight from the `main` branch root.
There is no build step. To host your own copy, fork the repo, then set
**Settings → Pages → Deploy from a branch → main / (root)**.

## Run it in a GitHub Codespace (alternative)

If you'd rather run it inside a Codespace instead of Pages:

```bash
npm install
npm start
```

Codespaces will pop up a "port forwarded" notification for port 3000 — click **Open in Browser**.
(`npm install` here just installs the tiny `serve` static-file server, since Codespaces needs
*something* to run — it's not a build step, this project has no framework or bundler.)

## Run it locally

You need a local server — opening `index.html` directly with `file://` will fail to load
`data.json` (browsers block that fetch for local files). Pick whichever is easiest:

**Option A — VS Code Live Server (easiest if you're already in VS Code)**
1. Open this folder in VS Code (`File > Open Folder…`)
2. Install the recommended **Live Server** extension when VS Code prompts you (or search
   "Live Server" by Ritwick Dey in the Extensions panel)
3. Right-click `index.html` → **"Open with Live Server"**
4. It opens at `http://127.0.0.1:5500`

**Option B — npm**
```bash
npm start
```
Then open `http://localhost:3000`.

**Option C — Python (if you have Python installed, no npm needed)**
```bash
python3 -m http.server 8000
```
Then open `http://localhost:8000`.

## Updating the data

`data.json` is a flat array of rows: `["COMPANY NAME", "STATE", total_petitions, "tier", "category"]`

- `tier`: `"L"` (Large), `"M"` (Mid), `"S"` (Small)
- `category`: `"construction"`, `"medical"`, `"it"`, `"eng"`, `"manufacturing"`, `"finance"`,
  `"trade"`, `"education"`, `"other"`

To refresh with a new USCIS export (e.g. the next fiscal year), regenerate `data.json` in this same
shape. The app itself doesn't need to change.

## Known limitations

- Field categorization is heuristic for the "Professional/Scientific/Technical Services" NAICS
  bucket (the largest one) — it's split by keyword matching on company name, so a handful of
  companies will land in the wrong category.
- "Scale" reflects H-1B petition volume only, not company revenue, total headcount, or non-H-1B hiring.
- Careers links are search links, not verified direct URLs — the source data doesn't include websites.
- Only one fiscal year of USCIS data is included; multi-year roll-ups are a possible extension.
