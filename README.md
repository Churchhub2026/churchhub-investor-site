# ChurchHUB.AI — Investor Website

Single-file, fully self-contained investor website for ChurchHUB.AI (Sophonix LLC).

## Contents
- **index.html** — the complete website. All assets (PDF prospectus, leadership
  photos, icons) are embedded as base64; there are **no external dependencies**,
  no web fonts, no CDN calls. Open it in any modern browser or serve it statically.

## Structure (preserved exactly)
- Five tabs: The Opportunity · Why ChurchHUB Wins · Financial Model · Leadership · Formulas
- Investor Deck modal (8 slides, keyboard nav)
- Investor Prospectus modal (embedded 32-page PDF, 90% viewport, pinned Download / Open-in-new-tab bar)
- Live financial model + 6-input stress engine with scenario presets
- TAM/SAM/SOM accordions, leadership cards, "Why This Team Works" banner

## Deploy
Static hosting. For GitHub Pages: Settings → Pages → deploy from `main` / root.
`.nojekyll` is included so the file is served verbatim.

This repository preserves the website exactly as generated. No redesign,
reframework, or recalculation has been applied.
