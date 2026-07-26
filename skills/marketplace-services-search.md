# Marketplace Services — `marketplace_services_search`

**MCP tool name:** `marketplace_services_search`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/marketplace-services/` (action: `search`)
**sv-cli command:** `sv marketplace-services` (aliases: marketplace, services)
**API reference:** [Marketplace Services](https://seovendor.co/api/marketplace-services.html)
**Type:** Synchronous

## What it does

Search the SV marketplace of digital marketing services and packages, with filters for price, series, and category.

## When to use this

Use when the agent (or the human it's assisting) is trying to find a fulfillable, done-for-you service on the SV marketplace rather than a self-serve API task.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `searchterm` | string | required | Search term, 1-200 characters. CLI flag is `--search` (not `--searchterm`) — maps directly to this field. |
| `price` | string | optional | Price ceiling — returns only services at or below this price. Omit or set 0 for no limit. |
| `series` | string | optional | Partial-match filter. Valid values: i-SERIES, S-SERIES, SC-SERIES, X-SERIES, XC-SERIES, M-SERIES, AX-SERIES, FUNNEL. |
| `category` | string | optional | Partial-match filter. Valid values: SEO, PPC, DEV. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options marketplace-services <field>` or `sv options marketplace-services <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show marketplace-services` shows the full current schema.

## Example request

```json
{
  "action": "search",
  "searchterm": "technical seo audit"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

Array of up to 5 matching services: `{Category, Series, Name, Term, Price, Subscription_Price, description, details_link}`.

## Chaining with other tools

Often the last step after a diagnostic tool (`preliminary_audit_analyze`, `geo_audit_create_task`) surfaces a gap the user wants done for them. Results are recommendations for a human to review/purchase, not something the agent should act on unilaterally.

## Gotchas

Capped at 5 results regardless of how many services match — narrow `searchterm`, `series`, or `category` if the first search feels too broad rather than expecting pagination.

