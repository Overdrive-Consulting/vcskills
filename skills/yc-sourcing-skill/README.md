# YC Sourcing Skill

Sources a Y Combinator batch and enriches it for investment review: pulls
companies from the YC directory with Apify, enriches founder emails and
LinkedIn, scrapes each website, finds recent press and momentum, and finds the
latest funding round. Everything merges into one record per company, with CSV
export.

```bash
npx vcskills-cli add yc-sourcing-skill
```

Then ask your agent: "Source W25 YC companies, B2B, hiring only."

Requires the Apify MCP server (`APIFY_TOKEN`) and the Exa MCP server
(`EXA_API_KEY`).
