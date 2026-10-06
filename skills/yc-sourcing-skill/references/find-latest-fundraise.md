# Find Latest Fundraise

**Pipeline:** [source-ycombinator](./source-ycombinator.md) → [enrich-founders](./enrich-founders.md) → [scrape-website](./scrape-website.md) → [find-latest-news](./find-latest-news.md) → **find-latest-fundraise** → [export-csv](./export-csv.md)

You are step 5 of 5. Your job is to find the latest known fundraising round for each YC company — round type, amount, date, and lead investors.

## Tools

- **Exa MCP (`web_search_exa`)** — primary: search for funding announcements
- **Exa MCP (`crawling_exa`)** — fetch Crunchbase or TechCrunch funding pages
- **Exa MCP (`company_research_exa`)** — deep company research including funding history
- **WebSearch / WebFetch** — fallback

## Input

Read `data/raw/yc_companies.json`. Use `company_name`, `website`, and `company_crunchbase` (if available) as search keys.

Note: All YC companies have received YC seed funding (standard $500K YC deal). Record this as the baseline. The goal is to find any additional rounds beyond YC.

## Step 1: Check Crunchbase directly

If the company has a `company_crunchbase` URL from the YC scraper, use it:

```
crawling_exa("[company_crunchbase]")
```

Extract:
- Funding rounds listed
- Round type (Pre-Seed, Seed, Series A, etc.)
- Amount raised
- Date
- Lead investors
- Total funding

## Step 2: Search for funding announcements

For each company (especially those without a Crunchbase URL or where Crunchbase returned nothing):

**Query 1 — Funding news:**
```
web_search_exa("[company_name] raises funding OR seed OR series A", date_range: last 24 months)
```

**Query 2 — Crunchbase lookup:**
```
web_search_exa("site:crunchbase.com [company_name] funding")
```

**Query 3 — Funding via company_research_exa:**
```
company_research_exa("[company_name]")
```

This often returns structured funding data including round amounts and investors.

Extract from any source:
- `round_type` — YC, Pre-Seed, Seed, Series A, Series B, Bridge, SAFE, etc.
- `amount_usd` — amount raised in USD (null if undisclosed)
- `announced_date` — date announced (ISO format, or year-only if exact date unknown)
- `lead_investors` — array of investor names
- `all_investors` — array of all participating investors
- `source_url` — URL where this was found
- `source_name` — e.g. "TechCrunch", "Crunchbase", "SEC filing"

## Step 3: Handle YC baseline

Every company should have at minimum the YC funding record:
```json
{
  "round_type": "YC",
  "amount_usd": 500000,
  "announced_date": "[batch year]",
  "lead_investors": ["Y Combinator"],
  "all_investors": ["Y Combinator"],
  "source_url": "[yc_profile_url]",
  "source_name": "Y Combinator"
}
```

Derive `announced_date` from the batch: W25 → 2025-01, S24 → 2024-07, etc.

## Step 4: Classify fundraising status

Based on all rounds found, set `fundraising_status`:

| Condition | Status |
|-----------|--------|
| Only YC funding, no subsequent rounds | `yc_only` |
| Seed or Pre-Seed raised post-YC | `seed_funded` |
| Series A raised | `series_a` |
| Series B or beyond | `growth_stage` |
| Bridge round or extension noted | `bridge` |
| Fundraising currently active (noted in recent news) | `actively_raising` |
| Unknown / no data found | `unknown` |

Also flag:
- `total_raised_usd` — sum of all known rounds (null if mostly undisclosed)
- `last_round_date` — date of most recent round (for sorting by recency)
- `has_post_yc_funding: true/false`

## Step 5: Assess investment opportunity signals

For each company, derive:
- `likely_raising_soon: true/false` — based on:
  - Last round was > 18 months ago AND company is early stage
  - Recent hiring news or expansion signals
  - YC-only and > 12 months post-graduation
- `notes` — 1–2 sentence summary of funding status for VC context

## Output

Write an array to `data/normalized/company_fundraises.json` conforming to `../schemas/company_fundraise.schema.json`.

One record per company, including `company_id` and `company_name` as join keys.

Also write a final combined output to `data/normalized/sourced_companies.json` that merges all pipeline data (from `yc_companies.json`, `enriched_founders.json`, `website_content.json`, `company_news.json`, and `company_fundraises.json`) into one comprehensive record per company.

## Review checkpoint

After writing all output files, present a final sourcing summary:

```
Sourcing pipeline complete. [N] YC companies fully enriched.

Funding breakdown:
  YC only (no post-YC raise): [N]
  Seed funded: [N]
  Series A+: [N]
  Unknown / no data: [N]

Likely raising soon: [N] companies flagged

Top opportunities by recency of last raise:
| # | Company | Batch | Stage | Last Round | Amount | Likely Raising |
|---|---------|-------|-------|-----------|--------|---------------|
| 1 | ... | W25 | Seed | 2024-11 | $3M | No |
...

Data is saved to data/normalized/sourced_companies.json

You can now:
  - Export all data to CSV: "export csv"
  - Filter by fundraising status: "show only yc_only companies"
  - Re-run news for a company: "re-search news for [company]"
  - Start a new batch: "reset and source [batch]"
```
