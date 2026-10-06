# Find Company Updates

**Pipeline:** [load-portfolio](./load-portfolio.md) → **find-company-updates** → [generate-lp-report](./generate-lp-report.md)

You are step 2 of 3. Your job is to research each portfolio company and surface the most recent and relevant updates — news coverage, funding activity, product milestones, and team changes — that an LP would care about.

## Tools

- **Exa MCP (`web_search_exa`)** — primary: search for recent news, funding, and coverage
- **Exa MCP (`crawling_exa`)** — fetch full content from Crunchbase, TechCrunch, and company pages
- **Exa MCP (`company_research_exa`)** — deep company research including structured funding data
- **WebSearch / WebFetch** — fallback for any source Exa doesn't cover

## Input

Read `data/raw/portfolio_companies.json`. Use `company_name` and `website` as search keys.
Read `data/raw/report_metadata.json` for the report period (use as the news date range if specified).

## Step 1: Determine date range

Use the report period from `report_metadata.json`:
- "Q1 2025" → search from 2024-10-01 to 2025-03-31
- "last 6 months" → last 6 months from today
- If no period specified → default to last 12 months

## Step 2: Research each company

For each company, run the following searches. Do companies in batches of 3–5 to respect rate limits.

### 2a. General news coverage

**Query 1 — Recent news:**
```
web_search_exa("[company_name]", date_range: [report_period])
```

**Query 2 — Product and launch news:**
```
web_search_exa("[company_name] launch OR product OR partnership OR milestone", date_range: [report_period])
```

**Query 3 — Founder/team news (for notable events):**
```
web_search_exa("[company_name] team OR hire OR CEO OR founder", date_range: [report_period])
```

### 2b. Funding research

**Query 4 — Funding announcements:**
```
web_search_exa("[company_name] raises OR funding OR round OR investment", date_range: last 24 months)
```

**Query 5 — Crunchbase:**
```
web_search_exa("site:crunchbase.com [company_name] funding")
```
If a Crunchbase URL is found, fetch it with `crawling_exa` for structured round data.

**Query 6 — Deep company research:**
```
company_research_exa("[company_name]")
```
This often returns structured funding history, headcount, and recent activity.

### 2c. Direct website check (if website available)

```
crawling_exa("[website]/blog") or crawling_exa("[website]/news")
```
Surface any recent product updates or announcements the company published directly.

## Step 3: Classify and extract updates

For each company, extract and classify all findings into these update types:

**News articles** (from queries 1–3):
- `title` — article headline
- `url` — source URL
- `source` — publication name (e.g. TechCrunch, Forbes, Bloomberg)
- `published_date` — ISO date
- `summary` — 1–2 sentence summary
- `update_type` — one of:
  - `funding` — fundraising announcement
  - `product_launch` — new product or major feature
  - `partnership` — enterprise deal or partnership
  - `hiring` — significant hiring or exec addition
  - `award` — award, recognition, or ranking
  - `founder_interview` — founder profile or press feature
  - `customer_win` — notable customer or case study
  - `expansion` — geographic or market expansion
  - `general` — general coverage or mention

Keep up to 6 most recent and relevant articles per company. Do not fabricate articles.

**Funding rounds** (from queries 4–6):
- `round_type` — Pre-Seed, Seed, Series A, Series B, Bridge, SAFE, Strategic, etc.
- `amount_usd` — amount raised in USD (null if undisclosed)
- `announced_date` — ISO date or year
- `lead_investors` — array of investor names
- `all_investors` — array of all participating investors
- `source_url` — URL where this was found
- `source_name` — publication or database name

## Step 4: Score company momentum

For each company, assign a `momentum_score` from 0–5:

| Criteria | Score |
|----------|-------|
| No coverage or updates found | 0 |
| 1–2 older articles (> 6 months ago), no funding | 1 |
| Some coverage or 1 older funding round | 2 |
| Recent funding OR 3+ articles in period | 3 |
| Recent funding AND product/partnership news | 4 |
| Major fundraise + significant press + product traction | 5 |

Also flag:
- `new_funding_in_period: true/false` — funding announced during report period
- `product_launched_in_period: true/false` — product launch during period
- `key_hire_in_period: true/false` — significant exec or team hire during period

## Step 5: Write one-paragraph narrative per company

Based on all findings, write a 2–4 sentence `update_narrative` for each company — the kind of brief that an LP would read in a report. Focus on:
- What happened in the period (funding, product, traction)
- What it signals about trajectory
- Any concerns or open questions (flag clearly if uncertain)

Keep it factual. If there's nothing to report, say so honestly: "No significant public updates were found for [company] during this period."

## Output

Write an array to `data/normalized/company_updates.json`.

One record per company, structured as:

```json
{
  "company_id": "acme-corp",
  "company_name": "Acme Corp",
  "website": "acme.com",
  "stage": "Seed",
  "sector": "B2B SaaS",
  "investment_date": "2023-03",
  "momentum_score": 4,
  "new_funding_in_period": true,
  "product_launched_in_period": false,
  "key_hire_in_period": true,
  "update_narrative": "Acme Corp closed a $5M seed extension in February 2025 led by Benchmark...",
  "funding_rounds": [...],
  "news_articles": [...],
  "latest_funding": {
    "round_type": "Seed Extension",
    "amount_usd": 5000000,
    "announced_date": "2025-02",
    "lead_investors": ["Benchmark"]
  }
}
```

## Review checkpoint

After writing `data/normalized/company_updates.json`, present a summary:

```
Researched [N] portfolio companies for [report_period]:

Momentum overview:
  High momentum (4–5): [N] companies
  Active (2–3): [N] companies
  Quiet / no updates (0–1): [N] companies

Funding activity:
  New rounds announced: [N] companies
  Total capital raised in period: $[sum] (where disclosed)

Key signals:
  Product launches: [N]
  Key hires: [N]
  Major press coverage: [N]

Top movers:
| Company | Momentum | Funding | Key Update |
|---------|----------|---------|-----------|
| Acme Corp | ⬆ 5 | $5M Seed Ext | Benchmark-led round, new VP Sales |
...

Companies with no updates: [list]

Does this look right? You can:
  - Confirm to proceed to step 3 (generate LP report)
  - Re-research a company: "re-research [company name]"
  - Add a note manually: "add note for [company]: [text]"
  - Flag a company for special attention: "flag [company]"
```

Do not proceed to `generate-lp-report` until the VC confirms.
