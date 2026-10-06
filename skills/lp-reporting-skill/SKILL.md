---
name: lp-reporting-skill
description: >
  Generate an LP (limited partner) report from a list of portfolio companies.
  Loads the portfolio, researches each company's recent news, funding rounds,
  product launches and key hires with Exa, scores momentum, and writes a
  formatted markdown report with an executive summary, per-company updates and a
  momentum table. Use when the user asks to "generate an LP report", "portfolio
  update", "quarterly LP update", "research my portfolio companies", "draft an
  LP letter", or mentions LP reporting or portfolio monitoring.
---

# LP Reporting

A three-step pipeline that turns a list of portfolio companies into an LP report.
Each step has its own instructions in `references/`; read the step's file when
you reach it, and stop for a review checkpoint after each one.

```
load-portfolio → find-company-updates → generate-lp-report
```

| Step | Instructions | Output |
|------|--------------|--------|
| 1. Load the portfolio | [references/load-portfolio.md](references/load-portfolio.md) | `data/raw/portfolio_companies.json`, `data/raw/report_metadata.json` |
| 2. Find company updates | [references/find-company-updates.md](references/find-company-updates.md) | `data/normalized/company_updates.json` |
| 3. Generate the report | [references/generate-lp-report.md](references/generate-lp-report.md) | `data/normalized/lp_report.md` |

## Requirements

- **Exa MCP** (`web_search_exa`, `crawling_exa`, `company_research_exa`) for news,
  funding and company research. WebSearch / WebFetch are the fallback.
- `EXA_API_KEY` set for the Exa MCP server.

## Rules

- Ask clarifying questions only when missing information would change the research.
- Never fabricate news, funding amounts or milestones. Label inferred or uncertain data.
- Present a review checkpoint after each step and wait for the user before moving on.
- Batch Exa searches (3–5 companies at a time) to respect rate limits.

## Starting

Begin with step 1: ask which portfolio companies to include, plus the report
period, audience (LPs, board, internal) and fund name if the user has them.
