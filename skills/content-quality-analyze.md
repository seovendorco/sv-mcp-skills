# Content Quality — `content_quality_analyze`

**MCP tool name:** `content_quality_analyze`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/content-quality/` (action: `analyze`)
**sv-cli command:** `sv content-quality` (aliases: quality, hcu-quality, eeat-quality)
**API reference:** [Content Quality](https://seovendor.co/api/content-quality.html)
**Type:** Synchronous

## What it does

Score a page's content quality against Google Helpful Content (HCU) and E-E-A-T signals for a target keyword — optionally comparing two URLs head to head.

## When to use this

Use to score a specific piece of content — typically right after drafting (`seogpt2_create_task`) or on an existing published page.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `kw` | string | required | Keyword text, 4-300 characters. |
| `url1` | string | required | Website URL to evaluate, max 300 characters. |
| `url2` | string | optional | A second URL for a head-to-head comparison, max 300 characters. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options content-quality <field>` or `sv options content-quality <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show content-quality` shows the full current schema.

## Example request

```json
{
  "action": "analyze",
  "kw": "white label seo",
  "url1": "https://example.com"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

Array of up to 15 entries per URL (30 total when `url2` is set): `{Webpage (1 or 2), URL, Question (1-15, an E-E-A-T question number), Score (0-100), Reason}`. Each entry is one E-E-A-T question evaluated for one URL.

## Chaining with other tools

`seogpt2_create_task` → `content_quality_analyze` → (revise if scores are weak) → `core_analysis_analyze` is a solid draft-to-publish quality gate.

## Gotchas

Requires a live, published URL (`url1`) — check whether unpublished drafts need to be temporarily hosted before scoring, since there's no raw-text input option unlike `content_transformer_rewrite`. Treat scores as a signal to investigate per-question via `Reason`, not a single pass/fail number.

