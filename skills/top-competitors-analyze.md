# Top Competitors — `top_competitors_analyze`

**MCP tool name:** `top_competitors_analyze`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/top-competitors/` (action: `analyze`)
**sv-cli command:** `sv top-competitors` (aliases: competitors)
**API reference:** [Top Competitors](https://seovendor.co/api/top-competitors.html)
**Type:** Synchronous

## What it does

Return the top 10 competitor URLs currently ranking for a keyword.

## When to use this

Use early in a competitive-analysis pipeline to identify who to actually benchmark against.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `kw` | string | required | Keyword text, 4-100 characters. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options top-competitors <field>` or `sv options top-competitors <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show top-competitors` shows the full current schema.

## Example request

```json
{
  "action": "analyze",
  "kw": "white label seo"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

Array of `{link, ranking (1=top), self (1 if this URL belongs to the queried site, else 0)}`.

## Chaining with other tools

`top_competitors_analyze` → `insight_igniter_entities` (per competitor) → `core_analysis_analyze` is a common sequence for building a competitive brief.

## Gotchas

No `web`/`url` field on this endpoint at all — `self` is how the API flags whether a result is the caller's own site, inferred server-side rather than passed in.

