# Preliminary Audit — `preliminary_audit_analyze`

**MCP tool name:** `preliminary_audit_analyze`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/preliminaryaudit/` (action: `analyze`)
**sv-cli command:** `sv preliminary-audit` (aliases: prelim-audit)
**API reference:** [Preliminary Audit](https://seovendor.co/api/preliminaryaudit.html)
**Type:** Synchronous

## What it does

Generate an essential technical SEO and GEO health report for a URL: SSL, redirects, canonical tags, robots.txt, sitemap, crawlability, load speed.

## When to use this

Use as a first-pass technical diagnostic before deeper competitive or content work — catch technical blockers early.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `URL` | string | required | Target URL to audit. Note the capitalized field name — this tool uses `URL`, not `url`. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options preliminaryaudit <field>` or `sv options preliminaryaudit <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show preliminaryaudit` shows the full current schema.

## Example request

```json
{
  "action": "analyze",
  "URL": "https://example.com"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

`data.overallscore`/`maxscore` plus a fixed set of named checks, each shaped `{score, maxscore, message, label, help}` (and sometimes `URL`/`retcode`/`checkedurl`/`redirectedurl`): `providedurl`, `effectiveurl`, `apiscore` (crawl/index difficulty), `blockingmetatags`, `canonical`, `LoadSpeed`, `NoSSL`/`NoSSLwww`, `SSL`/`SSLwww`, `matchurl`, `consistenturl`, `homeurl`, `robots`, `crawl`, `sitemap`.

## Chaining with other tools

Good starting point before `core_analysis_analyze` or a content pipeline — catch technical blockers before investing in content strategy. Lighter-weight than `geo_audit_create_task`, which is the deeper async GEO audit.

## Gotchas

Only tool in the set whose sole required field is capitalized `URL` (not `url` or `web`) — easy to miss when scripting across multiple tools with a shared payload builder.

