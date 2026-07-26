# Better Keywords AI — `better_keywords_research`

**MCP tool name:** `better_keywords_research`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/better-keywords/` (action: `research / filter`)
**sv-cli command:** `sv better-keywords` (aliases: keywords)
**API reference:** [Better Keywords AI](https://seovendor.co/api/better-keywords.html)
**Type:** Synchronous

## What it does

Discover high-intent keywords with volume, CPC, competition, and search-intent scoring — or AI-filter an existing keyword set down to the terms that matter (`filter` action).

## When to use this

Usually the *first* tool in a content pipeline — identify and validate target keywords before generating content, planning topics, or running competitive analysis.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `kw` | string | required | Seed keyword text, 4-300 characters (used by `research`; also required as a field even for `filter`, per the schema). |
| `brand` | string | optional | Brand text, max 60 characters. |
| `web` | string | optional | Website URL, max 300 characters — when provided, results include `rank`/`relativeurl` showing whether the site already ranks for each keyword. |
| `url` | string | optional | Fallback website URL used when `web` is not sent. |
| `location` | string | optional | Location text, max 50 characters. |
| `brandpersona` | string | optional | Brand persona text, max 2000 characters. |
| `lang` | integer | optional | Language ID, same 40-language set. |
| `researchtype` | array | optional | ~65 options steering research angle (Product Interest, Local SEO Focus, Competitive Analysis, B2B Marketing, etc). |
| `kwcompetition` | array | optional | ~32 options filtering by competition profile (Opportunity Keywords, Low Competition, High-ROI, Zero Competition, Buyer Intent, etc). |
| `data` | array | conditional | **Required for `filter` action only.** Array of keyword objects to AI-filter, max 1000 items. Each object needs `keyword` (string); optional `competition`, `volume`, `score`, `category`, `intent`, `cpc`. Pass the array returned by a prior `research` call directly — there's no CLI flag for this field, it requires a raw JSON call. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options better-keywords <field>` or `sv options better-keywords <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show better-keywords` shows the full current schema.

## Example request

```json
{
  "action": "research",
  "kw": "white label seo",
  "web": "https://example.com",
  "brand": "Acme SEO"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

`research`: array of keyword objects with `keyword`, `category`, `cpc`, `competition` (0-100), `volume`, `rank` (0 if not ranked), `intent`, `score`, `relativeurl`, plus provenance fields (`keyword_source`, `metrics_source`, `metrics_status`, `research_origin`, `fanout_parent_keyword`, `retrieved_at`, `score_formula`, etc). `filter`: array of `{Keyword, DTR}` — only rows scoring DTR ≥ 80 are returned; a submitted row missing `keyword` is dropped entirely.

## Chaining with other tools

`better_keywords_research` → `topical_authority_topics` → `seogpt2_create_task` is the standard research-to-draft pipeline. Also feeds `ranklens_rank` and `top_competitors_analyze` for competitive framing.

## Gotchas

`filter` has no CLI flag for its `data` field — it must be called as a raw JSON payload (`sv call better-keywords --json '...'`), typically piping in the array from a prior `research` call. The `filter` response shape (`Keyword`/`DTR`, capital K) is different from the `research` response shape (`keyword` lowercase, richer fields) — don't assume they're interchangeable when parsing.

