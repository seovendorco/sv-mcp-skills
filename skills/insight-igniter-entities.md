# Insight Igniter — `insight_igniter_entities`

**MCP tool name:** `insight_igniter_entities`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/insight-igniter/` (action: `entities`)
**sv-cli command:** `sv insight-igniter` (aliases: insights)
**API reference:** [Insight Igniter](https://seovendor.co/api/insight-igniter.html)
**Type:** Synchronous

## What it does

Extract the entities, topics, and relationships a page is associated with across traditional search and generative engines.

## When to use this

Use to understand what entities a page is already associated with — before writing competing/complementary content, or before a `topical_authority_topics` pass, to avoid duplicating entity coverage.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `web` | string | required | Website URL, max 300 characters. |
| `url` | string | optional | Fallback website URL used when `web` is not sent. |
| `brand` | string | optional | Brand text, max 60 characters. |
| `lang` | integer | optional | Language ID, same 40-language set. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options insight-igniter <field>` or `sv options insight-igniter <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show insight-igniter` shows the full current schema.

## Example request

```json
{
  "action": "entities",
  "web": "https://example.com",
  "brand": "Acme SEO"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

`data.results` — array, one entry per AI engine queried, each with `engine` (ID) and `result` (array of `{entity, category, confidence, intent, demand, engine}`). `data.errors` — engines that failed, with `engine`/`code`/`message`. `meta.engines` lists engines used.

## Chaining with other tools

Often run against competitor URLs pulled from `top_competitors_analyze`, feeding into `topical_authority_topics` or `core_analysis_analyze`.

## Gotchas

This tool queries multiple AI engines per call and can partially fail — always check `data.errors` even when `success: true`, since some engines may have returned nothing.

