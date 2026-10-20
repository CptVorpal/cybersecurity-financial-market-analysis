---
name: cyber-industry-viz
description: Build or refresh a single-file, five-tab HTML visualization of the cybersecurity industry landscape (threats, market size, key vendors, venture capital and M&A, defense nexus) where every number is tied to a cited source. Use when asked to create, update or refresh a cybersecurity industry dashboard or landscape page, including a periodic data refresh with a dated output file.
---

# Cybersecurity Industry Landscape Visualization

Produce a general-purpose HTML page. Do NOT add audience, presenter, session-date or location references.

## Global rule: sourcing
Every number on the page must come from a cited source found during this run. Never invent index scores, spend shares, category counts, country splits or forecasts. If a figure cannot be sourced, remove it or label it "n/a" / "not reported". Prefer primary sources (reports, press releases, SEC filings) over blogs restating them. Do not use line charts across unordered categories (companies, vectors).

## Paths
- `OUT_DIR`: the user's chosen output folder (default: current working directory).
- Template: `template/cybersecurity_industry_2026.html` inside this skill, if present. If absent, do a first build following the "Page spec" below, then use that output as the template on later runs.
- Before overwriting any existing file, show what will change and get confirmation. Never overwrite an older dated file.

## Steps
1. Get today's date: `date +%Y-%m-%d`.
2. Open the template and edit data in place; do not rebuild from scratch. Preserve the 5 tabs (Threat Landscape, Market & Scale, Key Players, Capital in Motion, Defense Nexus), the nation-state table sorted by severity, and the fixed-height wrapper on the threatSurface canvas.
3. The page has exactly 5 charts, each sourced:
   - **threatSurface**: single-series bars of share of breaches by vector from the latest Verizon DBIR (ransomware, third-party, credential abuse, vulnerability exploitation, phishing, misconfiguration/errors). No overlays or second axis. Update footnote and takeaway.
   - **playersChart**: bars of market cap / revenue multiple for vendors with a sourced market cap and revenue (e.g. CrowdStrike on ARR, Palo Alto, Fortinet, Zscaler). Compute from sourced figures, show inputs in bar labels, keep the explanatory note. Include Cloudflare or SentinelOne only if a sourced market cap is found; otherwise keep them on vendor cards with market cap "n/a". Vendor cards use latest reported fiscal-year/ARR or annualized quarterly revenue (labelled as such).
   - **maDeals**: top deals by value, built from the DEALS array (step 5).
   - **scaleGeo**: US share of global VC funding (latest Crunchbase figure) vs rest of world only; do not split other countries unless sourced.
   - **vcYoY**: VC metrics indexed to prior year = 100 using sourced figures only (total raised, number of rounds, Series A/B), from Pinpoint/Crunchbase.
   - Do not add: market growth, analyst-estimates, investment-priority, M&A-by-category, VC-by-sector or VC-by-stage charts unless rebuilt with fully sourced data.
4. **Market size**: one card per clearly scoped definition, no cross-year or cross-source comparisons. Narrow = latest Gartner end-user security spending; Broad = latest Cybersecurity Ventures total; plus McKinsey AI-expanded potential. State what each includes.
5. **M&A**: timeline and deals chart are date-driven. Append new deals to the DEALS array (fields: `d` ISO date, optional `label`, `acq`, `tgt`, `val` in $B or null, `note`, `tags` [[text, 1 if military]], optional `mega`). The page computes a rolling 12-month window from the viewer's date; never hardcode date ranges in headings. Pull deals from monthly SecurityWeek M&A roundups.
6. **Unicorns**: count card is "Private Cybersecurity Unicorns" (Failory list; note it may include since-acquired companies). Re-verify the count and every unicorn card valuation against a current-year funding announcement. Check each company's status at run time and exclude any that have been acquired or gone public. Show stage/date; replace or remove anything unconfirmed. Keep the funding rounds table current (company, amount, stage and date, category).
7. Use WebSearch in parallel to refresh with current-year figures only (no stale baselines): cybercrime cost, IBM breach cost, attacks per organization (Check Point), workforce gap, ransomware share; recent activity for China, Russia, Iran, North Korea; revenue/ARR and market caps for Palo Alto, CrowdStrike, Fortinet, Zscaler, Cloudflare, SentinelOne; latest quarterly VC totals. Mark acquired companies as acquired. Arrows follow the metric's direction (up for rising, down for falling); color red only when the change is bad. Flag any Defense Nexus figure (DoD cyber budget, CMMC contractor count, Zero Trust target year, JWCC value) that cannot be confirmed from a source.
8. Update "Data as of" / "Last updated" dates and footer sources.
9. Save to `OUT_DIR/cybersecurity_industry_YYYY-MM-DD.html` (today's date), then copy the identical file to `OUT_DIR/cybersecurity_industry_<year>.html`.
10. Verify: both files exist; 5 tab sections; `node --check` passes on the extracted script; every canvas id has a matching chart in the script and vice versa. Report which figures changed, which could not be refreshed, and which items were removed because they could not be sourced.

## Page spec (first build, no template)
Single self-contained HTML file, Chart.js from a pinned CDN version, five tab sections with the names above, five `<canvas>` ids (threatSurface inside a fixed-height wrapper, playersChart, maDeals, scaleGeo, vcYoY), a `DEALS` array driving the M&A timeline and chart, a nation-state table sorted by severity, vendor cards, unicorn cards, funding rounds table, and a footer listing every source with access date.
