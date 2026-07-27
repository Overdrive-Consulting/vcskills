# VCSkills

The curated skill library for venture capital — 93 agent skills for GPs,
analysts, and operators. Browse them at [vcskills.io](https://vcskills.io).

## Install

```bash
npx vcskills-cli add <skill-name>
```

Or use the Skills CLI directly against this repo:

```bash
npx skills add Overdrive-Consulting/vcskills --skill <skill-name>
```

## What's in here

Each skill lives in `skills/<name>/` and contains its full contents — `SKILL.md`
plus any `references/`, `scripts/`, or supporting files the original ships.

Every skill also carries:

- **`SOURCE.md`** — the upstream repository, the exact commit mirrored, the path
  within that repo, the declared license, and a note of any change we made.
- **`LICENSE-UPSTREAM`** — the upstream license text, verbatim, where the source
  repository declares one.

The only modification made to mirrored content is the `name` field in each
`SKILL.md` frontmatter, set to the skill's name in this library so
`--skill <name>` resolves. Each `SOURCE.md` records the original value.

## Attribution and licensing

**Skill content belongs to its original authors.** This repository redistributes
it with attribution; it does not relicense it. The `LICENSE` file in this
repository covers only our own contributions — the index, the README, and the
directory structure. Any given skill is governed by its own upstream license, as
recorded in its `SOURCE.md` and `LICENSE-UPSTREAM`.

Licenses across the library:

| License | Skills |
|---|---|
| Apache-2.0 | 43 |
| MIT | 33 |
| none declared | 11 |
| CC-BY-SA-4.0 | 3 |
| NOASSERTION | 2 |
| MPL-2.0 | 1 |

`none declared` means the upstream repository has no license file. Those skills
are mirrored with attribution and a link to the original.

**If you are the author of a mirrored skill** and want it removed, or want its
attribution corrected, please [open an issue](https://github.com/Overdrive-Consulting/vcskills/issues).
We will act on it promptly — no justification needed.

## The library

| Skill | License | Upstream |
|---|---|---|
| [`ai-product-skill`](skills/ai-product-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills/tree/main/skills/ai-product) |
| [`analyzing-funding-landscape-skill`](skills/analyzing-funding-landscape-skill/) | none declared | [jesseotremblay/claude-skills](https://github.com/jesseotremblay/claude-skills/tree/main/analyzing-funding-landscape) |
| [`architecture-diagram-creator-skill`](skills/architecture-diagram-creator-skill/) | Apache-2.0 | [mhattingpete/claude-skills-marketplace](https://github.com/mhattingpete/claude-skills-marketplace) |
| [`audit-context-building-skill`](skills/audit-context-building-skill/) | CC-BY-SA-4.0 | [trailofbits/skills](https://github.com/trailofbits/skills) |
| [`audit-support-skill`](skills/audit-support-skill/) | Apache-2.0 | [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins/tree/main/finance/skills/audit-support) |
| [`aws-skills`](skills/aws-skills/) | MIT | [zxkane/aws-skills](https://github.com/zxkane/aws-skills) |
| [`board-prep-skill`](skills/board-prep-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/board-prep) |
| [`business-analytics-reporter-ai-labs`](skills/business-analytics-reporter-ai-labs/) | MIT | [ailabs-393/ai-labs-claude-skills](https://github.com/ailabs-393/ai-labs-claude-skills/tree/main/dist/skills/business-analytics-reporter) |
| [`call-prep-skill`](skills/call-prep-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/GTM/skills/call-prep) |
| [`churn-prediction-skill`](skills/churn-prediction-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/CX/skills/churn-prediction) |
| [`claude-d3js-skill`](skills/claude-d3js-skill/) | none declared | [chrisvoncsefalvay/claude-d3js-skill](https://github.com/chrisvoncsefalvay/claude-d3js-skill) |
| [`claude-scientific-skills`](skills/claude-scientific-skills/) | MIT | [K-Dense-AI/claude-scientific-skills](https://github.com/K-Dense-AI/claude-scientific-skills) |
| [`cloudflare-skill`](skills/cloudflare-skill/) | MIT | [dmmulroy/cloudflare-skill](https://github.com/dmmulroy/cloudflare-skill) |
| [`code-auditor-skill`](skills/code-auditor-skill/) | Apache-2.0 | [mhattingpete/claude-skills-marketplace](https://github.com/mhattingpete/claude-skills-marketplace) |
| [`company-narrative-skill`](skills/company-narrative-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/company-narrative) |
| [`competitive-intelligence-market-research-skill`](skills/competitive-intelligence-market-research-skill/) | none declared | [sundial-org/awesome-openclaw-skills](https://github.com/sundial-org/awesome-openclaw-skills/tree/main/skills/competitive-intelligence-market-research) |
| [`competitive-intelligence-skill`](skills/competitive-intelligence-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/GTM/skills/competitive-intelligence) |
| [`competitive-landscape-analysis-skill-wshobson`](skills/competitive-landscape-analysis-skill-wshobson/) | MIT | [wshobson/agents](https://github.com/wshobson/agents/tree/main/plugins/startup-business-analyst/skills/competitive-landscape) |
| [`competitor-intelligence-skill-founder`](skills/competitor-intelligence-skill-founder/) | MIT | [ognjengt/founder-skills](https://github.com/ognjengt/founder-skills/tree/main/skills/competitor-intel) |
| [`contract-review-skill-cuad`](skills/contract-review-skill-cuad/) | MIT | [evolsb/claude-legal-skill](https://github.com/evolsb/claude-legal-skill) |
| [`copywriting-skill`](skills/copywriting-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills) |
| [`creating-financial-models-skill-cookbooks`](skills/creating-financial-models-skill-cookbooks/) | MIT | [anthropics/claude-cookbooks](https://github.com/anthropics/claude-cookbooks/tree/main/skills/custom_skills/creating-financial-models) |
| [`dashboard-creator-skill`](skills/dashboard-creator-skill/) | Apache-2.0 | [mhattingpete/claude-skills-marketplace](https://github.com/mhattingpete/claude-skills-marketplace) |
| [`data-and-metrics-skill`](skills/data-and-metrics-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/data-and-metrics) |
| [`data-storytelling-skill`](skills/data-storytelling-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills/tree/main/skills/data-storytelling) |
| [`deep-research-skill`](skills/deep-research-skill/) | none declared | [standardhuman/deep-research-skill](https://github.com/standardhuman/deep-research-skill) |
| [`demo-script-skill`](skills/demo-script-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/GTM/skills/demo-script) |
| [`differential-review-skill`](skills/differential-review-skill/) | CC-BY-SA-4.0 | [trailofbits/skills](https://github.com/trailofbits/skills) |
| [`expansion-playbook-skill`](skills/expansion-playbook-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/CX/skills/expansion-playbook) |
| [`financial-statements-skill`](skills/financial-statements-skill/) | Apache-2.0 | [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins/tree/main/finance/skills/financial-statements) |
| [`founder-coach-skill`](skills/founder-coach-skill/) | none declared | [acossta/chief-of-staff-oss](https://github.com/acossta/chief-of-staff-oss/tree/main/.claude/skills/founder-coach) |
| [`founder-market-research-skill`](skills/founder-market-research-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/market-research) |
| [`founder-playbook-skill`](skills/founder-playbook-skill/) | MIT | [tenequm/claude-plugins](https://github.com/tenequm/claude-plugins) |
| [`founder-sales-sprint-skill-lenny`](skills/founder-sales-sprint-skill-lenny/) | Apache-2.0 | [liqiongyu/lenny_skills_plus](https://github.com/liqiongyu/lenny_skills_plus/tree/main/skills/founder-sales) |
| [`fundraising-knowledge-skill`](skills/fundraising-knowledge-skill/) | none declared | [yamz8/open-ceo](https://github.com/yamz8/open-ceo/tree/main/fundraising/skills/fundraising-knowledge) |
| [`fundraising-process-skill`](skills/fundraising-process-skill/) | Apache-2.0 | [liqiongyu/lenny_skills_plus](https://github.com/liqiongyu/lenny_skills_plus/tree/main/skills/fundraising) |
| [`go-to-market-plan-skill-founder`](skills/go-to-market-plan-skill-founder/) | MIT | [ognjengt/founder-skills](https://github.com/ognjengt/founder-skills/tree/main/skills/go-to-market-plan) |
| [`gtm-strategy-skill`](skills/gtm-strategy-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/GTM/skills/gtm-strategy) |
| [`health-scoring-skill`](skills/health-scoring-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/CX/skills/health-scoring) |
| [`hiring-skill`](skills/hiring-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/hiring) |
| [`investor-management-skill`](skills/investor-management-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/investor-management) |
| [`investor-research-skill`](skills/investor-research-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/investor-research) |
| [`investor-updates-skill`](skills/investor-updates-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/investor-updates) |
| [`kpi-dashboard-design-skill`](skills/kpi-dashboard-design-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills/tree/main/skills/kpi-dashboard-design) |
| [`launch-strategy-skill`](skills/launch-strategy-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills/tree/main/skills/launch-strategy) |
| [`lean-startup-methodology-skill`](skills/lean-startup-methodology-skill/) | none declared | [rwHiveAqua/_idea_spinner](https://github.com/rwHiveAqua/_idea_spinner/tree/main/.claude/skills/lean-startup) |
| [`legal-risk-assessment-skill`](skills/legal-risk-assessment-skill/) | Apache-2.0 | [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins/tree/main/legal/skills/legal-risk-assessment) |
| [`market-sizing-analysis-skill`](skills/market-sizing-analysis-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills/tree/main/skills/market-sizing-analysis) |
| [`marketing-competitive-analysis-skill`](skills/marketing-competitive-analysis-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Marketing/skills/competitive-analysis) |
| [`meeting-briefing-skill`](skills/meeting-briefing-skill/) | Apache-2.0 | [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins/tree/main/legal/skills/meeting-briefing) |
| [`metrics-review-skill`](skills/metrics-review-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/metrics-review) |
| [`monthly-investor-updates-skill`](skills/monthly-investor-updates-skill/) | none declared | [yamz8/open-ceo](https://github.com/yamz8/open-ceo/tree/main/investor-updates/skills/monthly-investor-updates) |
| [`obra-superpowers`](skills/obra-superpowers/) | MIT | [obra/superpowers](https://github.com/obra/superpowers) |
| [`outreach-specialist-skill-founder`](skills/outreach-specialist-skill-founder/) | MIT | [ognjengt/founder-skills](https://github.com/ognjengt/founder-skills/tree/main/skills/outreach-specialist) |
| [`pitch-deck-builder-ai-labs`](skills/pitch-deck-builder-ai-labs/) | MIT | [ailabs-393/ai-labs-claude-skills](https://github.com/ailabs-393/ai-labs-claude-skills/tree/main/dist/skills/pitch-deck) |
| [`pitch-deck-skill`](skills/pitch-deck-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/pitch-deck) |
| [`plan-creation-skill`](skills/plan-creation-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/plan-creation) |
| [`pricing-strategist-skill-founder`](skills/pricing-strategist-skill-founder/) | MIT | [ognjengt/founder-skills](https://github.com/ognjengt/founder-skills/tree/main/skills/pricing-strategist) |
| [`pricing-strategy-skill`](skills/pricing-strategy-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills) |
| [`product-competitive-analysis-skill`](skills/product-competitive-analysis-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Product/skills/competitive-analysis) |
| [`product-led-growth-skill`](skills/product-led-growth-skill/) | MIT | [wdavidturner/product-skills](https://github.com/wdavidturner/product-skills/tree/main/skills/product-led-growth) |
| [`product-manager-toolkit-skill`](skills/product-manager-toolkit-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills/tree/main/skills/product-manager-toolkit) |
| [`product-market-fit-assessment-skill`](skills/product-market-fit-assessment-skill/) | NOASSERTION | [slgoodrich/agents](https://github.com/slgoodrich/agents/tree/main/plugins/ai-pm-copilot/skills/product-market-fit) |
| [`product-marketing-skill`](skills/product-marketing-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Marketing/skills/product-marketing) |
| [`product-metrics-tracking-skill`](skills/product-metrics-tracking-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Product/skills/metrics-tracking) |
| [`prompt-engineer-skill`](skills/prompt-engineer-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills) |
| [`quarterly-business-review-skill`](skills/quarterly-business-review-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/CX/skills/prepare-quarterly-business-review) |
| [`rag-engineer-skill`](skills/rag-engineer-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills) |
| [`reconciliation-skill`](skills/reconciliation-skill/) | Apache-2.0 | [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins/tree/main/finance/skills/reconciliation) |
| [`roadmap-management-skill`](skills/roadmap-management-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Product/skills/roadmap-management) |
| [`roi-calculator-skill`](skills/roi-calculator-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/GTM/skills/roi-calculator) |
| [`sales-forecast-skill`](skills/sales-forecast-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/GTM/skills/forecast) |
| [`sales-investor-emails-skill`](skills/sales-investor-emails-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/sales-investor-emails) |
| [`sales-playbook-skill`](skills/sales-playbook-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/GTM/skills/sales-playbook) |
| [`seo-audit-skill`](skills/seo-audit-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills) |
| [`stakeholder-comms-skill`](skills/stakeholder-comms-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Founder/skills/stakeholder-comms) |
| [`startup-analyst-skill`](skills/startup-analyst-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills/tree/main/skills/startup-analyst) |
| [`startup-financial-modeling-skill`](skills/startup-financial-modeling-skill/) | MIT | [sickn33/antigravity-awesome-skills](https://github.com/sickn33/antigravity-awesome-skills/tree/main/skills/startup-financial-modeling) |
| [`startup-metrics-framework-skill`](skills/startup-metrics-framework-skill/) | MIT | [wshobson/agents](https://github.com/wshobson/agents) |
| [`startup-pivoting-skill`](skills/startup-pivoting-skill/) | Apache-2.0 | [liqiongyu/lenny_skills_plus](https://github.com/liqiongyu/lenny_skills_plus/tree/main/skills/startup-pivoting) |
| [`startup-validator-comprehensive-market-analysis`](skills/startup-validator-comprehensive-market-analysis/) | MIT | [ailabs-393/ai-labs-claude-skills](https://github.com/ailabs-393/ai-labs-claude-skills/tree/main/dist/skills/startup-validator) |
| [`static-analysis-skill`](skills/static-analysis-skill/) | CC-BY-SA-4.0 | [trailofbits/skills](https://github.com/trailofbits/skills) |
| [`strategic-pitch-optimization-skill`](skills/strategic-pitch-optimization-skill/) | none declared | [samarv/Shanon](https://github.com/samarv/Shanon/tree/main/.claude/skills/strategic-pitch-optimization) |
| [`strategic-planning-skill-founder`](skills/strategic-planning-skill-founder/) | MIT | [ognjengt/founder-skills](https://github.com/ognjengt/founder-skills/tree/main/skills/strategic-planning) |
| [`stripe-best-practices-skill`](skills/stripe-best-practices-skill/) | MIT | [stripe/ai](https://github.com/stripe/ai) |
| [`terraform-code-generation-skill`](skills/terraform-code-generation-skill/) | MPL-2.0 | [hashicorp/agent-skills](https://github.com/hashicorp/agent-skills) |
| [`timeline-creator-skill`](skills/timeline-creator-skill/) | Apache-2.0 | [mhattingpete/claude-skills-marketplace](https://github.com/mhattingpete/claude-skills-marketplace) |
| [`us-market-bubble-detector-skill`](skills/us-market-bubble-detector-skill/) | MIT | [tradermonty/claude-trading-skills](https://github.com/tradermonty/claude-trading-skills/tree/main/skills/us-market-bubble-detector) |
| [`user-research-synthesis-skill`](skills/user-research-synthesis-skill/) | Apache-2.0 | [propane-ai/kits](https://github.com/propane-ai/kits/tree/main/plugins/Product/skills/user-research-synthesis) |
| [`variance-analysis-skill`](skills/variance-analysis-skill/) | Apache-2.0 | [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins/tree/main/finance/skills/variance-analysis) |
| [`vc-market-sizing`](skills/vc-market-sizing/) | NOASSERTION | [luisschmitzheadline/VC-Skills.md](https://github.com/luisschmitzheadline/VC-Skills.md) |
| [`willingness-to-pay-discovery-skill`](skills/willingness-to-pay-discovery-skill/) | none declared | [samarv/Shanon](https://github.com/samarv/Shanon/tree/main/.claude/skills/willingness-to-pay-discovery) |
| [`yc-startup-fundamentals-skill`](skills/yc-startup-fundamentals-skill/) | none declared | [jona/ycombinator-skills](https://github.com/jona/ycombinator-skills/tree/main/skills/yc-startup-fundamentals) |
