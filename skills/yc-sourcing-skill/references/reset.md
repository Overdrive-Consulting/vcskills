# Reset Pipeline Data

Delete data files so the sourcing pipeline can be re-run from scratch or from a specific step.

## Files by step

| Step | Files produced | Path |
|------|---------------|------|
| 1 — source-ycombinator | yc_companies.json | `data/raw/` |
| 2 — enrich-founders | enriched_founders.json | `data/normalized/` |
| 3 — scrape-website | website_content.json | `data/raw/` |
| 4 — find-latest-news | company_news.json | `data/normalized/` |
| 5 — find-latest-fundraise | company_fundraises.json, sourced_companies.json | `data/normalized/` |

## Reset modes

Ask the VC which reset they want before doing anything:

```
What would you like to reset?

  1. Full reset — delete all pipeline data (start completely fresh)
  2. From step 2 — keep YC company list, re-run enrichment onwards
  3. From step 3 — keep YC companies + founder enrichment, re-run website scraping onwards
  4. From step 4 — keep everything through website scraping, re-run news onwards
  5. From step 5 — keep everything through news, re-run fundraise lookup only

Reply with the step number or "full".
```

Wait for the VC to reply before deleting anything.

## Files to delete per mode

| Mode | Files to delete |
|------|----------------|
| Full reset (1) | All files listed above |
| From step 2 | enriched_founders.json, website_content.json, company_news.json, company_fundraises.json, sourced_companies.json |
| From step 3 | website_content.json, company_news.json, company_fundraises.json, sourced_companies.json |
| From step 4 | company_news.json, company_fundraises.json, sourced_companies.json |
| From step 5 | company_fundraises.json, sourced_companies.json |

## Execution

After the VC confirms the reset mode, run the corresponding shell commands.

**Full reset:**
```bash
rm -f data/raw/yc_companies.json \
       data/raw/website_content.json \
       data/normalized/enriched_founders.json \
       data/normalized/company_news.json \
       data/normalized/company_fundraises.json \
       data/normalized/sourced_companies.json
```

**From step 2:**
```bash
rm -f data/normalized/enriched_founders.json \
       data/raw/website_content.json \
       data/normalized/company_news.json \
       data/normalized/company_fundraises.json \
       data/normalized/sourced_companies.json
```

**From step 3:**
```bash
rm -f data/raw/website_content.json \
       data/normalized/company_news.json \
       data/normalized/company_fundraises.json \
       data/normalized/sourced_companies.json
```

**From step 4:**
```bash
rm -f data/normalized/company_news.json \
       data/normalized/company_fundraises.json \
       data/normalized/sourced_companies.json
```

**From step 5:**
```bash
rm -f data/normalized/company_fundraises.json \
       data/normalized/sourced_companies.json
```

Only delete files that actually exist — skip any that are already missing without error.

## After reset

Confirm to the VC what was deleted and what step to run next:

```
Reset complete. Deleted:
  - [list of deleted files]

Kept:
  - [list of kept files]

Next step: run [step name] to continue.
```
