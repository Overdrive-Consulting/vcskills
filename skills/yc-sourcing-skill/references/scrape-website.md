# Scrape Website

**Pipeline:** [source-ycombinator](./source-ycombinator.md) → [enrich-founders](./enrich-founders.md) → **scrape-website** → [find-latest-news](./find-latest-news.md) → [find-latest-fundraise](./find-latest-fundraise.md) → [export-csv](./export-csv.md)

You are step 3 of 5. Your job is to fetch and extract meaningful content from each company's website — product description, value proposition, target customer, and key differentiators.

## Tools

- **Exa MCP (`crawling_exa`)** — primary: fast content extraction for most sites
- **Apify MCP (`apify/website-content-crawler`)** — fallback for JS-heavy SPAs that Exa can't render
- **WebFetch** — lightweight fallback for simple static pages

## Input

Read `data/raw/yc_companies.json`. Use the `website` field for each company.

If `website` is null for a company, skip it and note it as `no_website`.

## Step 1: Prioritize and batch

Group companies:
- Companies with a `website` → scrape
- Companies without a `website` → skip, set `scrape_status: "no_website"`

For large batches (> 20 companies), process in groups of 10 to avoid rate limits. Inform the VC if this is the case.

## Step 2: Scrape each website

For each company with a website:

**Try Exa first** (`crawling_exa`):
```
crawling_exa("[company_website]")
```

Extract from the homepage:
- Page title
- Main headline / hero text
- Product description (what it does)
- Target customer / market
- Key value proposition or differentiator
- Any pricing signals or tiers mentioned
- Technology stack signals (if visible)

If Exa returns < 200 characters of useful content (e.g. blocked by JS), **fall back to Apify**:
```
apify/website-content-crawler with startUrls: [{ "url": "[company_website]" }], maxCrawlPages: 1
```

If both fail (blocked, 404, empty), set `scrape_status: "failed"` and move on.

## Step 3: Extract structured signals

From the raw scraped text, extract:

```
headline: <main hero headline>
product_description: <1–3 sentences on what the product does>
target_customer: <who it's for — e.g. "enterprise sales teams", "solo developers">
value_proposition: <key differentiator or benefit>
pricing_signals: <any pricing tiers, "free", "enterprise", etc. — null if none>
tech_signals: <technology stack or AI/ML signals visible on site — null if none>
```

If the website is a coming-soon page or under construction, set all fields to null and `scrape_status: "placeholder"`.

## Step 4: Cross-check with YC description

Compare extracted `product_description` with the company's `short_description` and `long_description` from the YC scraper.

If the website content significantly contradicts the YC description (e.g. different product entirely), flag with `content_mismatch: true`.

## Output

Write an array to `data/raw/website_content.json` conforming to `../schemas/company_website_content.schema.json`.

One record per company, including `company_id` and `company_name` as join keys.

## Review checkpoint

After writing `data/raw/website_content.json`, present a summary:

```
Scraped [N] company websites:

  ✓ Successfully scraped: [N]
  ✗ Failed / blocked: [N] — [list company names]
  ~ Placeholder / coming soon: [N]
  - No website: [N]

Content mismatches flagged: [N]

Sample:
| Company | Headline | Target Customer | Pricing Signal |
|---------|---------|----------------|---------------|
| ... | ... | ... | freemium |
...

Does this look right? You can:
  - Confirm to proceed to step 4 (find latest news)
  - Re-scrape a specific company: "re-scrape [company name]"
  - Manually add content: "set description for [company name] to [text]"
```

Do not proceed to `find-latest-news` until the VC confirms.
