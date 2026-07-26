# Topical Authority — `topical_authority_topics`

**MCP tool name:** `topical_authority_topics`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/topical-authority/` (action: `topics / content`)
**sv-cli command:** `sv topical-authority` (aliases: topical)
**API reference:** [Topical Authority](https://seovendor.co/api/topical-authority.html)
**Type:** Synchronous

## What it does

Plan topical authority by generating the article topics (and, via the `content` action, a fuller content plan) a site needs to own a subject.

## When to use this

Use once keyword research is done and it's time to plan a cluster of interlinked articles that build topical authority, rather than a single one-off piece.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `kw` | string | required | Keyword text, 4-300 characters. |
| `brand` | string | optional | Brand text, max 60 characters. |
| `web` | string | optional | Website URL, max 300 characters. |
| `url` | string | optional | Fallback website URL used when `web` is not sent. |
| `topicmode` | string | optional | `seo` (find SEO topics) or `geo` (find GEO topics) — changes the shape of the `content` action's response. |
| `brandpersona` | string | optional | Brand persona text, max 2000 characters. |
| `lang` | integer | optional | Language ID, same 40-language set. |
| `topicsize` | integer | optional | 0=10, 1=20, 2=30 topics returned. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options topical-authority <field>` or `sv options topical-authority <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show topical-authority` shows the full current schema.

## Example request

```json
{
  "action": "topics",
  "kw": "white label seo",
  "web": "https://example.com",
  "brand": "Acme SEO"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

`topics` action: array of `{subheading}` — suggested topic/subtopic titles. `content` action: array whose shape depends on `topicmode` — `seo` mode returns `{Topic, Source (0=AI-generated/1=Related Questions/2=Related Searches), Length, Date, subheading}`; `geo` mode returns just `{subheading}` entries.

## Chaining with other tools

`better_keywords_research` → `topical_authority_topics` → `seogpt2_create_task` (repeated per planned topic) is the standard content-cluster pipeline.

## Gotchas

Treat `topics` output as a plan, not finished content — each planned topic still needs its own `seogpt2_create_task` call. The `content` action's response shape genuinely differs by `topicmode` — don't assume `Topic`/`Length`/`Date` fields are present in `geo` mode.

