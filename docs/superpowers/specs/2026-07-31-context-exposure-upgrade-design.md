# stock-research upgrade — market/macro context + gap fixes

**Date:** 2026-07-31 · **Status:** approved design, pending implementation plan
**Branch:** `local/context-upgrade` (local divergence from upstream `MarginallyHarmless/stock-research`; PR-able later)

## Goal

Two things, one coherent pass:

1. **Put the company in its larger context.** The skill currently analyzes the company as if in a vacuum. Add (a) an unscored *weather* snapshot — market regime, rates, sector performance, live political/regulatory currents — where the reader asks "de ce a scăzut?", and (b) a scored *exposure* pillar — how structurally sensitive this company is to that weather. Split chosen deliberately: weather changes weekly and must never be scored (violates "aceleași culori indiferent de zi", scorecard.md); exposure is a slow company property and scores like the other pillars.
2. **Fix the gaps found in review** (see "Fixes" below), all small text edits.

**Unchanged invariants:** Romanian, concept-before-number, no invented figures (`N/A (nu apare pe sursă)`), no buy/sell/price-target language, free sources only, no API keys, single self-contained HTML report.

**New hard rules introduced by this upgrade:**
- Describing where rates/markets *are* is allowed; predicting where they *go* is banned, exactly like price targets.
- Every macro claim carries a date.
- News/search results are narrative, never a source of figures (existing rule, restated for the new searches).
- Current weather never changes the exposure color — only slow company properties do.

## User decisions (recorded)

- Macro section does **both**: unscored weather + scored exposure ("split roles").
- Scope: **fix everything** found in review, not just the macro addition.
- Placement: **Approach B** — weather folds into §1; exposure is a new scored §11 before Riscuri.

## Report structure after the change

Sections 1–10 keep their numbers. New **§11 Expunerea la lume — vânt din față sau din spate?** (scored, pillar #8). Old §11 Riscuri → **§12**; old §12 Ce ai învățat → **§13**. Scorecard: 7 → **8 pillars** (new row: "Expunere la lume"). Riscuri keeps *idiosyncratic* risks; systematic/macro exposure lives in §11 — no duplication.

## File-by-file changes

### 1. SKILL.md

**Frontmatter description** — append trigger phrases: "cum o afectează dobânzile / tarifele / alegerile", "contextul pieței", macro-context asks about a named company. Keep additions short; description is already long.

**Faza 2 (fetch table)** — add:

| # | URL | Ce scoți |
|---|---|---|
| 10 | `stockanalysis.com/stocks/{t}/financials/ratios/` | Multipli istorici pe an (P/E, P/FCF, EV/EBITDA) — pentru „sub media proprie pe 5 ani" din Evaluare |
| 11 | `finviz.com/groups.ashx` | Performanța sectoarelor (YTD, 1 an) — pentru comparația acțiune vs. sector vs. indice |

**Faza 2 — val doi (new subsection).** After wave 1 reveals industry/peers:
- Identify 2–3 **real niche competitors** (from the company page industry + judgment; naming may use judgment, **numbers may not** — every figure in the §8 competitor table must come from a fetched page). Fetch their `stockanalysis.com/stocks/{peer}/statistics/` in parallel.
- WebSearch, same wave: (a) 10Y Treasury yield now + ~12-month direction; (b) `{company}` + antitrust / tariffs / export controls / regulation, scoped to recent months. Both under the hard rules above.

**§1 (Faza 4) — element 5, „Vremea pieței":** where the S&P 500 sits vs. its 52W range (reuses SPY fetch #9), 10Y yield + direction (dated), sector YTD vs. S&P 500, and 1–2 dated narrative sentences on live political/regulatory currents touching this company. One sentence separating tide from swimmer, cross-referencing §11. Fix in element 4: headlines come from **finviz's news table (fetch #6)** — the stockanalysis root page was referenced but never fetched.

**§11 (Faza 4) — new section spec.** Four sub-assessments, judged separately like the moat types:
1. **Sensibilitatea evaluării la dobânzi** — how much of today's price is far-future profit (forward P/E level, FCF-negative status, net debt refinancing needs). Concept block teaches **multiple compression** — the centerpiece lesson: a growth stock can fall 40% while earnings grow, because P/E 60 → 35 when the risk-free rate rises.
2. **Ciclicitatea cererii** — discretionary vs. essential vs. mission-critical B2B; what a recession does to demand.
3. **Geografia veniturilor** — revenue by region (company page segments; 10-K if needed), tariff/export-control exposure, one-country concentration.
4. **Suprafața de reglementare** — active, *named* regulatory threads only; generic "reglementarea" stays banned.

**Faza 5** — reports save to the **current working directory**, explicitly not the skill directory. Same-day overwrite is fine.

**New subsection: „Varianta: două companii comparate."** One report, not two. Both tickers' Faza 2 in a single parallel wave (shared SPY/sector/rates). Scorecard table gets two semafor columns. Sections discuss both side by side; `lines` charts may carry both series (engine supports 3); returns-vs-index uses one `compare` block per company (the engine already renders each row against the index). Ends with **„diferențele care contează"** — 3–4 concrete contrasts, never "X e mai bună". Filename `{T1}-vs-{T2}-{AAAA-LL-ZZ}.html`.

### 2. references/scorecard.md

- **New §8 „Expunerea la lume"** (exceptions → §9, no-semafor → §10; TOC updated). Flag-based thresholds, mechanical like the rest of the file:
  - *Long-duration valuation flag:* forward P/E well above market (orientative ~35+) **or** FCF negative today.
  - *Cyclical demand flag:* demand evaporates in recessions.
  - *Geographic concentration flag:* one country/region ≳40% of revenue, or a politically sensitive supply chain (mirrors the "trei clienți = 40%" heuristic in Riscuri).
  - *Regulatory flag:* an active, named regulatory action with plausible material impact.
  - 🟢 zero flags · 🟡 one flag (or two partial) · 🔴 two-plus flags, or a concrete material regulatory threat.
  - Two philosophy guards written into the text: **exposure ≠ bad company** (red exposure next to six greens = "corabie excelentă pe mare agitată"; the lesson is position sizing and stomach, not avoidance), and **weather never changes this color** (colors move on ~annual timescales; cross-reference §9 exceptions for cyclicals).
- **§7 Sentiment fix** — green reworded around *flow direction*: net institutional inflows clearly positive over 12 months, short interest <3% of float, membership in major indices. Ownership *level* demoted to a context sentence (">60% e normalul la companiile mari — nu spune nimic; direcția banilor spune"), resolving the contradiction with scorecard.md:136.

### 3. references/glossary.md

Four new concepts (written in the file's existing register, English term in parens):
- **Comprimarea multiplilor (multiple compression)** — price falls while profits grow; link to the existing Microsoft-2000 capcană.
- **Vânt din față / din spate (headwind / tailwind)** — the tide vs. the swimmer.
- **Rotația de sector (sector rotation)** — why "nothing changed at the company but it fell 15%".
- **Dobânda fără risc (risk-free rate)** — the 10Y yield as gravity for all valuations, strongest on long-duration growth stocks.

One new capcană: **„Confuzia dintre valul sectorului și calitatea companiei"** — in a hot sector every boat rises; test the swimmer against the sector ETF, not just the S&P 500.

One new analogy in §3: rates as gravity; tide/swimmer.

### 4. references/data-sources.md

- Add `finviz.com/groups.ashx` entry (sector performance; map the company via its finviz "Sector" field). Verify the exact view/params with a live fetch during implementation; if unparseable, fall back to WebSearch and say so in the report.
- Add the second-wave peers pattern (peer `/statistics/` pages; naming by judgment, numbers only from fetches).
- Add rates sourcing: primary WebSearch (dated narrative); backup page `fred.stlouisfed.org/series/DGS10`.

### 5. assets/report-template.html

- §1: one extra `.stats` block (3 tiles: S&P 500 vs. max 52s · dobânda 10 ani SUA · sectorul YTD vs. S&P 500), each with dated `.s` subtext.
- New §11 section markup **from existing components only** (`.sec`, `.concept`, `.score` rows ×4: Dobânzi / Ciclu / Geografie / Reglementare); comments renumbered 11→12, 12→13.
- Scorecard: 8th row "Expunere la lume"; update the "restul:" comment.
- Cleanup: remove dead `.grp`/`.gc` CSS (engine never emits it).
- No engine changes, no new chart types, no new libraries.

### 6. README.md

- "Ce produce" table: new §11 row, renumbered 12/13; "șapte piloni" → "opt piloni".
- "Ce face diferit": one new bullet — puts the company in its macro/political context and scores *exposure*, never the weather.
- Install/usage/limits unchanged.

## Out of scope (deliberate)

No scored weather. No VIX or technical indicators. No FRED API dependency (page fetch at most). No template redesign. No recommendation language. No multi-series `bars` engine work.

## Acceptance criteria

1. All six files updated; section numbering consistent across SKILL.md, README, template (grep for "11", "12", "13", "șapte"/"opt" finds no stragglers).
2. Scorecard = 8 pillars everywhere; §11 sub-assessments match between SKILL.md, scorecard.md, and template.
3. No new paid/keyed dependency; fetch list grows by exactly 2 fixed URLs + peer pages + ≤2 WebSearches.
4. A live test run on one ticker (e.g. PLTR or NVDA) produces: weather tiles in §1 with dates, a scored §11 with four judged sub-rows, a competitor table whose every number traces to a fetched page, report saved in cwd.
5. The words „va scădea/va crește" about markets/rates appear nowhere; no buy/sell language; missing data rendered as `N/A (nu apare pe sursă)`.
6. Committed on `local/context-upgrade`; upstream `master` untouched.
