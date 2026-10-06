# Generate LP Report

**Pipeline:** [load-portfolio](./load-portfolio.md) → [find-company-updates](./find-company-updates.md) → **generate-lp-report**

You are step 3 of 3. Your job is to synthesize all company update data into a polished, readable LP report that a VC would be proud to send to their limited partners.

## Tools

- **Read / Write** — read pipeline data; write the final report

## Input

Read:
- `data/normalized/company_updates.json` — all enriched company data
- `data/raw/report_metadata.json` — fund name, period, audience

## Step 1: Determine report tone and detail level

Use `report_audience` from metadata to calibrate:

| Audience | Tone | Detail level |
|----------|------|-------------|
| LPs | Professional, measured, reassuring | High: include financials, context |
| Board | Direct, analytical | High: include metrics, risks |
| Internal | Casual, frank | Full: include flags and concerns |
| Not specified | Professional | Standard |

## Step 2: Compute portfolio-level stats

Before writing, compute:
- Total companies in portfolio
- Companies with updates in period vs. quiet
- Total capital raised across portfolio in period
- Number with new funding rounds
- Average momentum score
- Sectors represented
- Stage breakdown (Seed / Series A / etc.)

## Step 3: Write the report

Use the template below. Fill every section — do not leave placeholders. If data is missing for a section, write a brief honest note (e.g. "No significant updates were found for this period.") rather than omitting it.

---

### Report Template

```markdown
# [Fund Name] — Portfolio Update
**Period:** [Report Period]
**Date:** [Generated Date]
**Prepared for:** [Audience]

---

## Executive Summary

[3–5 sentence overview of the portfolio's performance and momentum this period.
Cover: how many companies had notable updates, total capital raised, standout
milestones, and the overall health signal. Be specific — name names. Avoid generic
"the portfolio continues to perform well" language.]

---

## Portfolio Highlights

**[N] companies** across **[sectors listed]** — **[N] with notable updates** this period.

| Metric | Value |
|--------|-------|
| Total companies | N |
| New funding rounds | N |
| Capital raised (period) | $X (where disclosed) |
| Product launches | N |
| Key hires | N |
| High-momentum companies (4–5) | N |
| Quiet / no updates | N |

---

## Company Updates

[One section per company, sorted by momentum score descending — most active companies first.]

### [Company Name] · [Stage] · [Sector]

**Momentum:** [score]/5 · **Invested:** [investment_date]  
**Website:** [website]

[update_narrative — the 2–4 sentence summary from find-company-updates]

**Latest funding:**  
[If new_funding_in_period: "Raised [amount] [round_type] in [date], led by [investors]."  
Else if any funding history: "Last raised [amount] [round_type] in [date]."  
Else: "No public funding history found."]

**Notable coverage:**  
[List up to 3 most relevant news articles as bullet points:  
- [Source] ([date]): [title] — [1-sentence summary]  
If no coverage: "No significant press coverage found this period."]

---

[Repeat for each company]

---

## Portfolio Momentum Summary

| Company | Stage | Sector | Momentum | New Funding | Key Update |
|---------|-------|--------|----------|-------------|-----------|
[One row per company, sorted momentum descending]

---

## Companies to Watch

[2–4 companies worth flagging for LP attention — either positive (strong momentum,
major milestone) or cautionary (quiet, no updates in >12 months, known headwinds).
Write 1–2 sentences per company explaining why they're flagged.]

---

## Notes & Caveats

- This report is based on publicly available information gathered on [date].
- Funding amounts marked as undisclosed were not found in public filings.
- [Any other caveats relevant to this run]
```

---

## Step 4: Quality check before saving

Before writing the final file, review:
- Every company has an update section (no missing companies)
- No fabricated data — all funding and news must come from `company_updates.json`
- Funding figures are marked "(undisclosed)" where `amount_usd` is null
- "Companies to Watch" includes at least one cautionary flag (not just praise)
- Dates are consistent with the report period

## Output

Write the completed report to `data/normalized/lp_report.md`.

## Final summary

After writing the report, present:

```
LP report generated for [N] companies.

Report saved to: data/normalized/lp_report.md

Quick stats:
  Report period: [period]
  Total capital raised: $[amount] (where disclosed)
  Companies with updates: [N] / [total]
  High-momentum companies: [N]

You can:
  - Read the report: "show me the report"
  - Adjust the tone: "rewrite the executive summary for a board audience"
  - Focus on a company: "expand the section on [company name]"
  - Add a note: "add a note to [company]'s section: [text]"
  - Re-run research for a company: "go back and re-research [company name]"
```
