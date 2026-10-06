# LP Reporting Skill

Turns a list of portfolio companies into an LP report: loads the portfolio,
researches recent news, funding, product and team updates for each company with
Exa, scores momentum 0–5, and writes a markdown report with an executive
summary, per-company sections, a momentum table and companies to watch.

```bash
npx vcskills-cli add lp-reporting-skill
```

Then ask your agent: "Generate an LP report for Acme Corp, Beta Labs and Gamma AI."

Requires the Exa MCP server (`EXA_API_KEY`).
