# CORE Analysis — `core_analysis_analyze`

**MCP tool name:** `core_analysis_analyze`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/core-analysis/` (action: `analyze`)
**sv-cli command:** `sv core-analysis` (aliases: core)
**API reference:** [CORE Analysis](https://seovendor.co/api/core-analysis.html)
**Type:** Synchronous

## What it does

Run a CORE analysis of a page (title, meta, headings, body, URL structure, SSL, indexing, sitemap/robots) plus its competitive landscape.

## When to use this

Use after content is drafted or published, to benchmark it directly against the current top-ranked pages for a query.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `url` | string | optional | Website URL, max 300 characters. |
| `kw` | string | optional | Keyword text, max 100 characters. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options core-analysis <field>` or `sv options core-analysis <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show core-analysis` shows the full current schema.

## Example request

```json
{
  "action": "analyze",
  "url": "https://example.com",
  "kw": "white label seo"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

A dense nested object: `SiteResponse`/`LoadSpeed`, `URL` (SSL/authority checks), `keyword` (uniqueness score), `Title`/`Meta`/`H1`/`H2`/`H3`/`Body` (each with text, counts, emphasized-term %, and 'base term' frequency breakdowns), `IndexedInGoogle`, `Sitemap`, `Robots_txt`, and `TopRankedSites`.

## Chaining with other tools

Pairs with `top_competitors_analyze` (identify who to benchmark against) and often runs after `seogpt2_create_task` to validate a fresh draft before publishing.

## Gotchas

Both `url` and `kw` are individually optional per the schema, but a meaningful analysis needs at least one — an empty-body call will likely just return an error or a near-empty report. This is the deepest/most nested response shape of the 16 tools; when summarizing it for a human, don't just dump the raw structure — pull out the scores and `TopRankedSites` that actually drive a decision.

