# Chaffee Provides — redesign draft

A proposed rebuild of [ChaffeeProvides.org](https://chaffeeprovides.org) (Guidestone
Colorado's local-food directory for Chaffee County). **Prototype for review, hosted on GitHub Pages. Not the official site.** Only the map page (`find-food.html`) and provider detail pages are published; the root redirects to the map.
Every page carries a "Draft for review" banner.

The current-site inventory this draft was built from is `docs/CHAFFEE_SITE_INVENTORY.md`
in the main colorado-farm-trail repo (branch `feat/chaffee-provides-site`).

## Hosting

Published from this repo's `main` branch with GitHub Pages. Every page carries
`noindex` and a "Prototype for review" banner, and `robots.txt` blocks crawlers. It's
meant to be shared by link only.

The source of truth is the `chaffee-provides/` folder on the
`feat/chaffee-provides-site` branch of dbrown-creator/colorado-farm-trail. This repo is
a published copy of it (with the hosting banner and noindex added).

## Pages

| Page | Replaces on the current site | What's new |
|---|---|---|
| `index.html` | Home (unfiltered ~40-card grid) | Search, product/type tiles with counts, "coming up" dates, "open this month", latest stories |
| `find-food.html` | Food Access Map (last edited 2021) | Combined sources, filters (type, product, town, open this month, SNAP), shareable URLs (`?offering=dairy&town=Salida`), list + map, mobile toggle, Chaffee County boundary |
| `provider.html?id=` | `/provider/<slug>/` | Map, hours, season, directions, "checked on" badge, per-field source labels, related providers, suggest-an-update link |
| `food-assistance.html` | Image-only food-access PDF | Accessible HTML schedule, filter by day, today highlighted, map, printable, each entry sourced |
| `spotlight/` | Spotlight blog (9 posts, 2021–22) | All 9 carried over + 2 new draft posts; alt text on every image |
| `list-your-business.html` | Promote Your Services form | Adds hours, season and SNAP fields; draft submits via email |
| `about.html` | About + Contact | How the data stays current, live directory stats, full mailing address |

## Data

`data/providers.json` is generated. Don't edit it by hand:

```bash
python scripts/chaffee_site/build_site_data.py [--inputs <repo checkout>]
```

It combines:
- the Phase 2 statewide build (`source-data/phase2/co_farmers_markets_all_raw.csv`), which merges Chaffee Provides, Colorado Proud markets, CFMA, USDA directories and official-site checks, with reviewed dedup decisions applied;
- the live Colorado Proud farm data (`data/markets.json`);
- the per-provider verification results (`source-data/phase2/enrichment/results/`), for the "checked on" date and status.

The area is Chaffee County plus anything Chaffee Provides itself lists.

Hand-maintained files:
- `data/food-assistance.json`: the weekly schedule. Each entry names its source; re-check against the organizations' sites each season.
- `data/posts.json`: Spotlight posts.
- `data/chaffee-county.json`: county boundary from US Census TIGERweb (GEOID 08015), simplified.
- `index.html`: the `EVENTS` list (dated items only; past ones drop off automatically).

## Before this could go live

- **Blog full text.** The nine original posts appear here as title, date, image and a short summary linking to the original. Import the full text from Guidestone's WordPress export (Tools → Export) with their permission, and 301-redirect the old URLs, including the broken `…weve-got-you-covered/` alias.
- **Images** are hotlinked from chaffeeprovides.org for the draft. Copy them in at launch, resized: several originals are 2,000–2,560 px and slow the Spotlight grid.
- **Listing form** opens an email for now. Connect it to a reviewed intake, e.g. the Farm Trail's Google Form flow (`docs/BUSINESS_SUBMISSIONS.md`).
- **Category labels.** Chaffee Provides uses "Market" for shops as well as farmers' markets, and the pipeline maps it to "Farmers' Market". Scanga Meat Company and The Asian Palate are corrected in `source-data/phase2/overrides.csv`. Others, such as The Lettucehead Food Company (a grocery), still need the same treatment or a mapping fix.
- **Providers needing a human check.** 11 were "unknown" in the Oct 3, 2026 verification. The site shows these with a "couldn't confirm" badge. Examples:
  - Caring & Sharing: website down
  - The Grainery: TLS error
  - Chaffee Cares: empty site
  - Sweet Pea Farm: Facebook only
- **Basemap key.** Uses the Farm Trail's CARTO key. Get a separate key if this ships under its own domain.
