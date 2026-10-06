# Export CSV

Convert sourcing pipeline JSON files to CSV. Each file gets its own CSV saved in the same folder.

## Available exports

| File | CSV output | Contents |
|------|-----------|---------|
| `data/raw/yc_companies.json` | `data/raw/yc_companies.csv` | One row per company (raw YC data) |
| `data/normalized/enriched_founders.json` | `data/normalized/enriched_founders.csv` | One row per founder with enriched contact details |
| `data/raw/website_content.json` | `data/raw/website_content.csv` | One row per company with website signals |
| `data/normalized/company_news.json` | `data/normalized/company_news.csv` | One row per company with top news article flattened |
| `data/normalized/company_fundraises.json` | `data/normalized/company_fundraises.csv` | One row per company with latest round details |
| `data/normalized/sourced_companies.json` | `data/normalized/sourced_companies.csv` | Full merged output — one row per company |

## What to ask

If the VC does not specify which file(s) to export, ask:

```
Which files would you like to export to CSV?

  1. All available files
  2. yc_companies — raw YC company data
  3. enriched_founders — founders with emails and LinkedIn
  4. website_content — scraped website signals
  5. company_news — latest news and momentum
  6. company_fundraises — latest funding rounds
  7. sourced_companies — full merged output (recommended)

Reply with a number, a comma-separated list (e.g. "3,6"), or "all".
```

Only export files that exist. Skip and note any that are missing.

## CSV column definitions

### yc_companies.csv

```
company_id, company_name, yc_profile_url, short_description, batch,
industry, subindustry, stage, status, tags,
company_location, year_founded, team_size,
website, company_linkedin, company_x, company_crunchbase, company_github,
is_hiring, number_of_open_jobs
```

- `tags`: join array with ` | `
- Flatten `founders[]` into separate columns: `founder_1_name`, `founder_1_email`, `founder_1_linkedin`, etc. (up to 3 founders)

### enriched_founders.csv

```
company_id, company_name, founder_id, name, title, bio, bio_source,
email, email_status, email_tier, email_source,
linkedin, linkedin_source, x
```

### website_content.csv

```
company_id, company_name, website, scrape_status,
headline, product_description, target_customer,
value_proposition, pricing_signals, tech_signals,
content_mismatch
```

### company_news.csv

Flatten top article (most recent) into columns:

```
company_id, company_name, news_momentum,
product_hunt_featured, hacker_news_featured,
top_article_title, top_article_url, top_article_source,
top_article_date, top_article_signal_type, top_article_summary,
total_articles_found
```

### company_fundraises.csv

```
company_id, company_name, fundraising_status,
total_raised_usd, has_post_yc_funding, likely_raising_soon,
last_round_type, last_round_amount_usd, last_round_date,
last_round_lead_investors, last_round_all_investors,
last_round_source_url, notes
```

- Arrays: join with ` | `

### sourced_companies.csv (full merged output)

```
company_id, company_name, batch, industry, stage, status,
company_location, year_founded, team_size, website,
short_description, long_description,
company_linkedin, company_x, company_crunchbase,
is_hiring, number_of_open_jobs,
founder_1_name, founder_1_email, founder_1_email_tier, founder_1_linkedin,
founder_2_name, founder_2_email, founder_2_email_tier, founder_2_linkedin,
founder_3_name, founder_3_email, founder_3_email_tier, founder_3_linkedin,
website_headline, website_product_description, website_target_customer,
website_value_proposition, website_pricing_signals,
news_momentum, top_article_title, top_article_source, top_article_date,
fundraising_status, total_raised_usd, has_post_yc_funding,
last_round_type, last_round_amount_usd, last_round_date, last_round_lead_investors,
likely_raising_soon
```

## How to write the CSV

Always write the Python conversion script to a `.py` file first, then execute it. Never use a bash heredoc (`<<'EOF'`) to run Python inline — it triggers a false-positive safety warning due to curly braces in dict comprehensions.

**Correct pattern:**
1. Write the script to `scripts/export_csv.py` using the file-write tool
2. Run it: `python3 scripts/export_csv.py`
3. Delete the script after it runs (optional)

**Wrong pattern** (do not use):
```bash
python3 - <<'EOF'
...script with {curly braces}...
EOF
```

## CSV formatting rules

- Always include a header row
- Wrap any field containing commas, newlines, or double quotes in double quotes
- Escape internal double quotes as `""`
- Use UTF-8 encoding
- Null values → empty cell (not the string "null")

## After export

List each file written with its row count:

```
Exported:
  ✓ data/normalized/sourced_companies.csv — 47 rows
  ✓ data/normalized/enriched_founders.csv — 89 rows
  ✓ data/normalized/company_fundraises.csv — 47 rows

Skipped (file not found):
  - data/raw/website_content.json
```
