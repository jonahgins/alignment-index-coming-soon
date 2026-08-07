# thealignmentindex.org — coming soon

Static holding page for The Alignment Index. One interactive page
(`index.html`), a 404, and generated brand assets. No build step, no
dependencies.

## Deployment

Pushes to `main` deploy via GitHub Pages to https://thealignmentindex.org
(custom domain via `CNAME`, DNS at Name.com, HTTPS enforced). This
repo's local git config authenticates through `gh` (account:
jonahgins) — plain pushes just work from this folder.

## Signups

The form POSTs to a Google Apps Script web app that validates,
dedupes, and appends to a private Google Sheet (owned by
jonah@thealignmentindex.org). The script lives in the Sheet:
Extensions → Apps Script. **Edits do nothing until you create a new
deployment version**: Deploy → Manage deployments → pencil → Version:
"New version" → Deploy. The endpoint URL never changes.

Server and client enforce the same email rules: shape, ≤ 254
characters, and no Sheets-formula leading characters (`= + - @ '`).

## Regenerating assets

Favicons and the share card are rasterized from committed SVG sources:

    qlmanage -t -s 32 -o /tmp favicon.svg && mv /tmp/favicon.svg.png favicon-32.png
    qlmanage -t -s 1200 -o /tmp og-image.svg
    sips -c 630 1200 /tmp/og-image.svg.png --out og-image.png   # qlmanage pads square

The apple-touch-icon is a full-bleed variant (iOS applies its own
corner mask); its source is not committed — regenerate from:

    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
      <rect width="32" height="32" fill="#0f0f0f"/>
      <circle cx="16" cy="16" r="13" fill="#a3a3a3"/>
    </svg>

    qlmanage -t -s 180 -o /tmp touch-icon.svg && mv /tmp/touch-icon.svg.png apple-touch-icon.png
