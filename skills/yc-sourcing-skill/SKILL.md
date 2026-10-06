---
name: yc-sourcing-skill
description: >
  Source and enrich Y Combinator startups for a VC. Pulls a YC batch from the
  YC directory with Apify, enriches founder emails and LinkedIn profiles,
  scrapes each company's website, finds recent press and momentum signals,
  and finds the latest funding round, then merges everything into one record
  per company and exports to CSV. Use when the user asks to "source YC
  companies", "source the W25 batch", "find YC startups", "enrich YC
  founders", or wants a sourced, enriched list of YC companies.
---

# YC Sourcing

A five-step sourcing pipeline, plus export and reset. Each step has its own
instructions in `references/`; read the step's file when you reach it, and stop
for a review checkpoint after each one.

```
source-ycombinator → enrich-founders → scrape-website → find-latest-news → find-latest-fundraise → export-csv
```

| Step | Instructions | Output |
|------|--------------|--------|
| 1. Source a YC batch | [references/source-ycombinator.md](references/source-ycombinator.md) | `data/raw/yc_companies.json` |
| 2. Enrich founders | [references/enrich-founders.md](references/enrich-founders.md) | `data/normalized/enriched_founders.json` |
| 3. Scrape websites | [references/scrape-website.md](references/scrape-website.md) | `data/raw/website_content.json` |
| 4. Find latest news | [references/find-latest-news.md](references/find-latest-news.md) | `data/normalized/company_news.json` |
| 5. Find latest fundraise | [references/find-latest-fundraise.md](references/find-latest-fundraise.md) | `data/normalized/company_fundraises.json`, `data/normalized/sourced_companies.json` |
| Export to CSV | [references/export-csv.md](references/export-csv.md) | `*.csv` next to each JSON |
| Reset | [references/reset.md](references/reset.md) | deletes data files to re-run |

Data files are written under `data/` in the user's working directory. The JSON
schemas each step conforms to are in this skill's `schemas/` folder.

## Requirements

- **Apify MCP** with `APIFY_TOKEN`: `michael.g/y-combinator-scraper`,
  `icypeas_official/bulk-email-finder`, `apify/website-content-crawler`.
- **Exa MCP** with `EXA_API_KEY`: `people_search_exa`, `web_search_exa`,
  `company_research_exa`, `crawling_exa`.

## Rules

- Ask clarifying questions only when missing information would change the run.
- Use only public professional contact information. Label inferred emails as inferred.
- Never fabricate URLs, emails, funding amounts or news articles.
- Present a review checkpoint after each step and wait for the user before moving on.
- Batch Apify calls and Exa searches to respect rate limits.

## Starting

Begin with step 1: ask which YC batch and filters to use (industry, region,
hiring only), for example "W25 batch, B2B, hiring only".
