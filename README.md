# Leadify Templates

Centralized legal templates for all Leadify landing pages. Each landing page fetches templates from this repo via GitHub Pages.

## Structure

```
templates/
├── au/                         # Australia
│   ├── disclaimer/
│   │   └── default.txt         # Footer disclaimer (plain text)
│   ├── privacy/
│   │   └── default.html        # Privacy policy (HTML)
│   └── terms/
│       └── default.html        # Terms & conditions (HTML)
├── nz/                         # New Zealand
│   ├── disclaimer/
│   │   └── default.txt
│   ├── privacy/
│   │   └── default.html
│   └── terms/
│       └── default.html
└── README.md
```

## Placeholders

Each template uses these placeholders that get replaced at runtime by the landing page:

| Placeholder | Replaced With | Example |
|---|---|---|
| `COMPANY NAME` | `config.companyName` | Pathway Pal |
| `WEBSITE URL` | `window.location.origin` | https://conveyancing.pathway-pal.com |
| `CTA TEXT` | `config.ctaText` | SUBMIT |
| `PRIVACY` | Link to `/privacy` page | `<a href='/privacy'>Privacy Policy</a>` |

## Usage

Landing pages fetch templates via GitHub Pages:

```js
// Example: fetching AU disclaimer
const response = await fetch(
  "https://leadify-au.github.io/templates/au/disclaimer/default.txt"
);
const text = await response.text();
const html = text
  .replaceAll("COMPANY NAME", config.companyName)
  .replaceAll("WEBSITE URL", websiteURL)
  .replaceAll("CTA TEXT", config.ctaText)
  .replaceAll("PRIVACY", privacyURL);
```

## How to Edit

1. Navigate to the file you want to edit on GitHub
2. Click the pencil icon to edit
3. Make your changes
4. Commit — changes go live automatically via GitHub Pages

## Adding Vertical-Specific Templates

If a vertical needs a different disclaimer (e.g. conveyancing vs auto loan), create a new file:

```
au/disclaimer/conveyancing.txt
au/disclaimer/auto-loan.txt
```

Then update the landing page's fetch URL to point to the specific file.
