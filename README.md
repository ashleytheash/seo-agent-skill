# SEO Agent Skill

Agent skills for SEO diagnosis, competitive analysis, blog production, and post-publish indexing automation.

Install a skill by copying its folder into your Codex/Cursor skills directory (for example `~/.codex/skills/<skill-name>/`).

## Skills

| Skill | Folder | Purpose |
| --- | --- | --- |
| **seo-optimization** | `seo-optimization/` | Diagnose SEO from GSC exports, indexing reports, and source code; prioritize structure, indexing, and CTR fixes |
| **gsc-seo-prioritizer** | `gsc-seo-prioritizer/` | Turn Search Console data into execution buckets: low impressions, high impressions/low CTR, unstable indexing, structural issues |
| **seo-competitive-analysis** | `seo-competitive-analysis/` | Analyze competitors as page systems; map compare/channel/problem/use-case opportunities to a page roadmap |
| **blog-indexing-automation** | `blog-indexing-automation/` | Design deployment-time indexing workflows: sitemap submission, URL Inspection monitoring, no unsupported "request indexing" automation |
| **open-design-blog-factory** | `open-design-blog-factory/` | Editorial blog generator for Open Design with search-intent-first structure and indexing cadence |
| **blog-factory** | `blog-factory/` | Business blog generator for nexu with GSC feedback loop, topical pillars, and content scoring |

## Recommended workflow order

1. **Strategy** — `seo-competitive-analysis` to decide which page types to build
2. **Diagnosis** — `gsc-seo-prioritizer` or `seo-optimization` to decide what to fix first
3. **Content** — `open-design-blog-factory` or `blog-factory` to produce posts
4. **Indexing** — `blog-indexing-automation` plus the Open Design GitHub Actions below

## Automation bundle (`automation/`)

Copy-ready scripts, GitHub Actions workflows, and operator docs from [nexu-io/open-design](https://github.com/nexu-io/open-design). See [automation/README.md](automation/README.md) for adoption steps, secrets, and `lib.ts` site configuration.

| Workflow | Bundle path | Schedule |
| --- | --- | --- |
| Blog SEO lint | `automation/workflows/landing-page-ci-seo.yml` | On PR |
| Post-deploy indexing | `automation/workflows/blog-indexing-on-deploy.yml` | After deploy |
| Indexing monitor | `automation/workflows/blog-indexing-monitor.yml` | Daily 02:00 UTC |
| Traffic digest | `automation/workflows/blog-3day-report.yml` | Daily 02:00 UTC |
| SEO daily report | `automation/workflows/seo-daily-report.yml` | Daily 01:00 UTC |

Docs:

- [automation/docs/blog-indexing-automation.md](automation/docs/blog-indexing-automation.md)
- [automation/docs/seo-daily-report.md](automation/docs/seo-daily-report.md)
- [automation/docs/examples/blog-indexing-status.md](automation/docs/examples/blog-indexing-status.md)

Scripts:

- `automation/scripts/blog-indexing/` (17 TypeScript modules)
- `automation/scripts/seo-daily-report.ts`

## Install example

```bash
git clone https://github.com/ashleytheash/seo-agent-skill.git
cp -R seo-agent-skill/seo-optimization ~/.codex/skills/
cp -R seo-agent-skill/gsc-seo-prioritizer ~/.codex/skills/
```

## License

Skill content follows the conventions of the projects they support. Check each target repo for product code licensing.
