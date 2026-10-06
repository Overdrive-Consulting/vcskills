# Enrich Founders

**Pipeline:** [source-ycombinator](./source-ycombinator.md) → **enrich-founders** → [scrape-website](./scrape-website.md) → [find-latest-news](./find-latest-news.md) → [find-latest-fundraise](./find-latest-fundraise.md) → [export-csv](./export-csv.md)

You are step 2 of 5. Your job is to enrich founder profiles for every company in `yc_companies.json` — filling missing LinkedIn URLs, emails, and bios.

## Tools

- **Exa MCP (`people_search_exa`)** — primary: find missing LinkedIn URLs and professional bios
- **Apify MCP (`icypeas_official/bulk-email-finder`)** — find missing founder emails by name + company domain
- **WebSearch / WebFetch** — fallback for checking company team pages

## Input

Read `data/raw/yc_companies.json`. If the file does not exist, stop and ask the VC to run `source-ycombinator` first.

Extract all founders across all companies. The YC scraper already provides:
- `name`, `title`, `bio` (from YC profile)
- `emails.email`, `emails.status` (from YC scraper — already high quality)
- `linkedin`, `x` (from YC profile — may be null)

## Step 1: Identify gaps

For each founder, flag which fields are missing:
- `linkedin` is null → needs Exa lookup
- `email` is null or `email_status` is `risky` → needs email finder
- `bio` is null or very short (< 50 chars) → needs Exa bio enrichment

Skip enrichment for founders who already have all fields populated.

## Step 2: Fill missing LinkedIn URLs via Exa

For founders missing a `linkedin` URL:

Use Exa MCP `people_search_exa`:
```
"[founder name]" "[company name]" founder OR CEO OR CTO
```

Extract the LinkedIn URL from the result. If multiple candidates, pick the one matching the company name.

If not found, leave `linkedin: null` — do not fabricate a URL.

Set `linkedin_source: "exa"` on any newly found LinkedIn URL.

## Step 3: Fill missing emails via Apify

For founders where email is null or risky, build a CSV for batch email lookup:

Extract the domain from the company's `website` field (e.g. `starsling.dev` from `https://www.starsling.dev/`). Strip `www.` and protocol.

Format the CSV:
```
firstName,lastName,domain
Sarah,Smith,starsling.dev
James,Lee,sequoia.com
```

Run Apify actor `icypeas_official/bulk-email-finder` with the prospect CSV.

The actor returns:
- `email` — found email address
- `confidence` — confidence score (0.0–1.0)
- `status` — `found`, `not_found`, or `risky`

Map results back to founders by `firstName + lastName + domain`.

Assign `email_tier`:
| Actor status | confidence | email_tier |
|---|---|---|
| `found` | ≥ 0.8 | `confirmed` |
| `found` | < 0.8 | `inferred` |
| `not_found` or `risky` | — | `not_found` |

Set `email_source: "icypeas"` for any found email from this step.

Note: Founders from the YC scraper with `email_status: "verified"` already have `email_tier: "confirmed"` — do not re-run the email finder for those.

## Step 4: Enrich missing bios via Exa

For founders with a short or missing bio:

Use Exa MCP `people_search_exa`:
```
"[founder name]" "[company name]"
```

Extract a 2–3 sentence professional bio from the result. Trim to relevant highlights (prior companies, roles, notable exits).

Set `bio_source: "exa"` on any newly added bio content.

## Step 5: Merge enriched data

For each founder, produce a merged record by:
1. Starting with the original YC data
2. Filling `linkedin` if previously null and found via Exa
3. Filling/upgrading `email` and `email_tier` if the bulk finder found a better result
4. Filling `bio` if previously short and Exa returned content

Do not overwrite confirmed YC-sourced emails with lower-confidence Exa results.

## Output

Update each company's `founders` array in place, then write the enriched dataset to `data/normalized/enriched_founders.json`.

Schema: `../schemas/founder_profile.schema.json`

Each founder record must include:
- `company_id`, `company_name` (from parent company)
- `founder_id`, `name`, `title`, `bio`, `bio_source`
- `email`, `email_status`, `email_tier`, `email_source`
- `linkedin`, `linkedin_source`
- `x`

## Review checkpoint

After writing `data/normalized/enriched_founders.json`, present a summary:

```
Enriched [N] founders across [M] companies:

Contact quality:
  ✓ Confirmed email (YC verified or high confidence): [N]
  ~ Inferred email (lower confidence): [N]
  ✗ No email found: [N]

  LinkedIn found: [N] / [total]
  Bio enriched via Exa: [N] founders

Sample:
| Company | Founder | Email | Tier | LinkedIn |
|---------|---------|-------|------|---------|
| ... | ... | ... | confirmed | yes/no |
...

Does this look right? You can:
  - Confirm to proceed to step 3 (scrape websites)
  - Correct a founder's email: "update email for [name] to [email]"
  - Remove a founder: "remove [name] from [company]"
```

Do not proceed to `scrape-website` until the VC confirms.
