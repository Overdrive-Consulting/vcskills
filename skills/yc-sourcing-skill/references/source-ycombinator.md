# Source YCombinator

**Pipeline:** **source-ycombinator** → [enrich-founders](./enrich-founders.md) → [scrape-website](./scrape-website.md) → [find-latest-news](./find-latest-news.md) → [find-latest-fundraise](./find-latest-fundraise.md) → [export-csv](./export-csv.md)

You are step 1 of 5. Your job is to pull a batch of YC companies from the YC directory using the Apify scraper and save the raw output.

## Tools

- **Apify MCP (`michael.g/y-combinator-scraper`)** — primary: scrape YC company directory with full company and founder data

## Input: Sourcing criteria

Ask the VC which YC batch(es) and filters they want. If not provided, ask:

```
Which YC companies would you like to source?

  Batch (e.g. W25, S24, X25 — or leave blank for all):
  Industry filter (e.g. B2B, Consumer, Fintech — or leave blank for all):
  Region filter (e.g. San Francisco, Remote — or leave blank for all):
  Hiring only? (yes / no / doesn't matter):
  Scrape founder details? (yes — recommended):

Reply with your preferences or just say "W25 batch, B2B, hiring only".
```

Wait for the VC to reply before running the scraper.

## Step 1: Build the scraper URL

Construct the YC directory search URL from the filters:

Base URL: `https://www.ycombinator.com/companies`

Append query parameters:
- Batch → `?batch=W25` (use YC batch code, e.g. W25, S24, X25, F24)
- Industry → `&industry=B2B` (URL-encode if needed)
- Region → `&regions=San%20Francisco%20Bay%20Area`
- Hiring → `&isHiring=true`

Examples:
- All W25 companies: `https://www.ycombinator.com/companies?batch=W25`
- W25 B2B hiring: `https://www.ycombinator.com/companies?batch=W25&industry=B2B&isHiring=true`
- All companies: set `scrape_all_companies: true` instead of providing a URL

## Step 2: Run the Apify scraper

Run the Apify actor `michael.g/y-combinator-scraper` with:

```json
{
  "url": "<constructed YC directory URL>",
  "scrape_all_companies": false,
  "scrape_founders": true,
  "scrape_open_jobs": false
}
```

If the VC wants all companies across all batches, use `scrape_all_companies: true` and omit `url`.

The actor returns an array of company objects. Each company includes:
- `company_id`, `company_name`, `url`, `short_description`, `long_description`
- `batch`, `industry`, `subindustry`, `stage`, `status`, `tags`
- `company_location`, `year_founded`, `team_size`
- `website`, `company_linkedin`, `company_x`, `company_crunchbase`, `company_github`
- `is_hiring`, `number_of_open_jobs`
- `founders[]` — array with `id`, `name`, `title`, `bio`, `emails.email`, `emails.status`, `linkedin`, `x`

## Step 3: Normalize and save

Map the raw Apify output to `../schemas/yc_company.schema.json` format.

Key mapping:
- `company_image` → `logo_url`
- `url` → `yc_profile_url`
- `emails.email` → `email`, `emails.status` → `email_status`, `emails.available` → `email_available`

Save the normalized array to `data/raw/yc_companies.json`.

## Review checkpoint

After writing `data/raw/yc_companies.json`, present a summary:

```
Sourced [N] YC companies from [batch/filter]:

Breakdown:
  Industry distribution: [top 3 industries with counts]
  Stage distribution: [Early: N, Growth: N, etc.]
  Hiring: [N] companies actively hiring
  Founders with email: [N] / [total founders]
  Founders with LinkedIn: [N] / [total founders]

Sample companies:
| # | Company | Batch | Industry | Stage | Founders | Website |
|---|---------|-------|----------|-------|---------|---------|
| 1 | ... | W25 | B2B | Early | 2 | example.com |
...

Does this look right? You can:
  - Confirm to proceed to step 2 (enrich founders)
  - Filter further: "only keep AI companies"
  - Remove a company: "remove [company name]"
  - Re-run with different filters
```

Do not proceed to `enrich-founders` until the VC confirms or adjusts the list.
