# Find Latest News

**Pipeline:** [source-ycombinator](./source-ycombinator.md) → [enrich-founders](./enrich-founders.md) → [scrape-website](./scrape-website.md) → **find-latest-news** → [find-latest-fundraise](./find-latest-fundraise.md) → [export-csv](./export-csv.md)

You are step 4 of 5. Your job is to find the latest press coverage, product announcements, and signals of momentum for each YC company.

## Tools

- **Exa MCP (`web_search_exa`)** — primary: find recent news, articles, and press coverage
- **Exa MCP (`crawling_exa`)** — fetch full article content when summary is insufficient
- **WebSearch / WebFetch** — fallback for any source Exa doesn't cover

## Input

Read `data/raw/yc_companies.json`. Use `company_name` and `website` as search keys.

## Step 1: Search for recent news per company

For each company, run at least 2 Exa searches targeting different news angles:

**Query 1 — General coverage:**
```
web_search_exa("[company_name] startup", date_range: last 12 months)
```

**Query 2 — Launch and product news:**
```
web_search_exa("[company_name] launch OR product OR raises", date_range: last 12 months)
```

**Query 3 — Founder press (optional, for high-signal companies):**
```
web_search_exa("[founder_name] [company_name]", date_range: last 24 months)
```

Prioritise sources: TechCrunch, Forbes, Business Insider, VentureBeat, The Information, Bloomberg, Reuters, Product Hunt, Hacker News.

## Step 2: Filter and score articles

For each article found, extract:
- `title` — article headline
- `url` — article URL
- `source` — publication name (e.g. TechCrunch)
- `published_date` — date published (ISO format)
- `summary` — 1–2 sentence summary of what the article says
- `signal_type` — classify the news type:
  - `funding` — fundraising announcement (skip here — handled by find-latest-fundraise)
  - `product_launch` — new product or feature launch
  - `partnership` — partnership or enterprise deal announced
  - `hiring` — significant hiring or team expansion
  - `award` — award, recognition, or ranking
  - `founder_interview` — founder profile or interview
  - `customer_win` — notable customer or case study
  - `general` — general coverage

Keep up to 5 most recent and relevant articles per company. Exclude pure funding announcements (those go in find-latest-fundraise).

If no articles are found, set `news: []` — do not fabricate coverage.

## Step 3: Derive momentum signals

Based on the news found, assign a `news_momentum` score:

| Criteria | Score |
|----------|-------|
| No coverage | 0 |
| 1–2 articles, > 6 months old | 1 |
| 1–2 articles, < 6 months old | 2 |
| 3+ articles in last 6 months | 3 |
| TechCrunch / major press in last 3 months | 4 |
| Multiple major press + product launch in last 3 months | 5 |

Also flag:
- `product_hunt_featured: true/false` — if Product Hunt coverage found
- `hacker_news_featured: true/false` — if HN Show/Ask coverage found

## Output

Write an array to `data/normalized/company_news.json` conforming to `../schemas/company_news.schema.json`.

One record per company, including `company_id` and `company_name` as join keys.

## Review checkpoint

After writing `data/normalized/company_news.json`, present a summary:

```
Found news coverage for [N] / [total] companies:

  No coverage: [N] companies
  Low momentum (1–2): [N] companies
  Strong momentum (3+): [N] companies
  Product Hunt featured: [N]
  Hacker News featured: [N]

Top stories by recency:
| Company | Source | Date | Signal |
|---------|--------|------|--------|
| ... | TechCrunch | 2025-04-12 | product_launch |
...

Does this look right? You can:
  - Confirm to proceed to step 5 (find latest fundraise)
  - Add a news article manually: "add article for [company name]: [url]"
  - Re-search a company: "re-search news for [company name]"
```

Do not proceed to `find-latest-fundraise` until the VC confirms.
