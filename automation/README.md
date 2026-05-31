# SEO automation bundle

Portable copy of the Open Design landing-page SEO automation: blog indexing after deploy, GSC monitoring, PR blog SEO checks, and a daily Search Console digest.

Reference implementation: [nexu-io/open-design](https://github.com/nexu-io/open-design).

## What's included

| Area | Location | Purpose |
| --- | --- | --- |
| Blog indexing scripts | `scripts/blog-indexing/*.ts` | Detect changed URLs, verify readiness, IndexNow, sitemap submit, URL Inspection, Search Analytics, status rendering, escalations, cross-post scaffold |
| GSC daily report | `scripts/seo-daily-report.ts` | Feishu card with site totals, movers, and optimization opportunities |
| Post-deploy indexing | `workflows/blog-indexing-on-deploy.yml` | Runs after static deploy; baseline inspection + analytics |
| Indexing monitor | `workflows/blog-indexing-monitor.yml` | T+1/T+3/T+7/T+14 re-inspection; stall/low-traffic issues |
| Traffic digest | `workflows/blog-3day-report.yml` | T-3 cohort + 30-day rolling traffic report PR |
| SEO daily report | `workflows/seo-daily-report.yml` | Scheduled GSC summary to Feishu |
| PR SEO health scan | `workflows/landing-page-ci-seo.yml` | `lint-blog-seo.ts` + `check-blog-url-changes.ts` on changed posts |
| Docs | `docs/` | Operator manuals; `docs/examples/` sample status/digest outputs |

## Adopt into a product repo

1. **Copy scripts** into your monorepo landing app (paths must match workflow commands):

   ```bash
   cp -R automation/scripts/blog-indexing apps/landing-page/scripts/
   cp automation/scripts/seo-daily-report.ts apps/landing-page/scripts/
   ```

2. **Copy workflows** to GitHub Actions (adjust `pnpm --filter` package name if needed):

   ```bash
   cp automation/workflows/*.yml .github/workflows/
   ```

3. **Wire deploy trigger**: `blog-indexing-on-deploy.yml` expects your static deploy workflow to complete successfully on `main` (see workflow `workflow_run` filters in the file).

4. **Configure site constants** in `apps/landing-page/scripts/blog-indexing/lib.ts`:

   - `SITE` — public origin (canonical URLs, IndexNow key URL)
   - `GSC_SITE_URL` — Search Console property (`sc-domain:example.com` or URL-prefix form)
   - `BLOG_DIR` — filesystem path to blog Markdown (default is relative to the landing app)

   `seo-daily-report.ts` reads the same GSC property via env / shared patterns; align domain with `GSC_SITE_URL`.

5. **Add repo docs** (optional): copy `automation/docs/blog-indexing-automation.md` and `seo-daily-report.md` to your repo `docs/`, and let bot PRs update `docs/blog-indexing-status.{md,json}` and `docs/blog-traffic-digest.md` at the repo root (workflows assume repo-root `docs/`).

6. **Secrets** in GitHub (repository or environment):

   | Secret | Used by |
   | --- | --- |
   | `GSC_SERVICE_ACCOUNT_KEY` | Indexing + daily report (unless OAuth mode) |
   | `GSC_OAUTH_CLIENT_ID` / `GSC_OAUTH_CLIENT_SECRET` / `GSC_OAUTH_REFRESH_TOKEN` | OAuth alternative to service account |
   | `BOT_APP_ID` / `BOT_APP_PRIVATE_KEY` | Bot PRs for status + traffic digest |
   | `FEISHU_WEBHOOK_URL` (+ optional `FEISHU_WEBHOOK_SECRET`) | `seo-daily-report`, optional digests |
   | `FEISHU_BLOG_DIGEST_WEBHOOK` | `blog-3day-report` optional push |
   | `DEVTO_API_KEY` / `HASHNODE_TOKEN` / `HASHNODE_PUBLICATION_ID` | Optional cross-post (manual publish only) |

7. **PR SEO**: enable `landing-page-ci-seo.yml` or merge its steps into existing `landing-page-ci.yml`.

## Layout in this repo

```
automation/
  README.md
  scripts/
    blog-indexing/
    seo-daily-report.ts
  workflows/
  docs/
    blog-indexing-automation.md
    seo-daily-report.md
    examples/
```

Operator docs: [docs/blog-indexing-automation.md](docs/blog-indexing-automation.md), [docs/seo-daily-report.md](docs/seo-daily-report.md).
