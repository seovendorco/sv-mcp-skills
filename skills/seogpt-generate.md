# SEO GPT — `seogpt_generate`

**MCP tool name:** `seogpt_generate`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/seogpt/` (action: `generate`)
**sv-cli command:** `sv seogpt` (aliases: seo-gpt)
**API reference:** [SEO GPT](https://seovendor.co/api/seogpt.html)
**Type:** Synchronous

## What it does

Generate SEO and GEO content — titles, descriptions, FAQs, posts, and more — from a single keyword across 300+ content types (`type` currently runs 0-331; the docs list 332 numbered types).

## When to use this

Reach for this when the agent needs one specific, usually shorter-form piece of on-brand content — a meta description, a title, an FAQ answer, a social post — rather than a full long-form article (use `seogpt2_create_task` for that).

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `kw` | string | required | Keyword text, 4-300 characters. |
| `type` | integer | required | Content type ID. 332 options, e.g. 0=Title, 8=Page Title, 18=Meta Description, 26=FAQ. **Do not guess** — confirm via `sv options seogpt type` or `sv seogpt types --search <term>`; via MCP, ask the agent to discover valid types before calling. |
| `brand` | string | optional | Brand text, max 60 characters. |
| `web` | string | optional | Website URL, max 300 characters. |
| `url` | string | optional | Fallback website URL used when `web` is not sent, max 300 characters. |
| `brandpersona` | string | optional | Brand persona text, max 2000 characters. |
| `length` | integer | optional | Length preset ID (e.g. 10, 20, 50, 100, 200, 300 words). See `sv options seogpt length`. |
| `qty` | integer | optional | Quantity ID, 1-10 items. |
| `lang` | integer | optional | Language ID, 40 supported languages (0=US English, 3=Spanish, etc). See `sv options seogpt --search` or `sv seogpt languages`. |
| `engine` | integer | optional | AI engine ID, 0-8 (Mu Engine Smart Writing, Rho Engine GPT-5.1/5.4, GPT-5.6, Gemini 3.1 Pro, Claude Sonnet 4.6, Perplexity Sonar, Grok 4.3, Llama 4 Maverick). |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options seogpt <field>` or `sv options seogpt <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show seogpt` shows the full current schema.

## Example request

```json
{
  "action": "generate",
  "kw": "white label seo",
  "type": 18,
  "web": "https://example.com",
  "brand": "Acme SEO"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

`data.text` — a single string of generated content (plain text or HTML depending on type). Synchronous, HTTP 200.

## Chaining with other tools

Typically the last step in a chain: `better_keywords_research` → `topical_authority_topics` → `seogpt_generate` (fill in specific content pieces from the plan).

## Gotchas

With 332 content types, never hard-code a type ID from memory in a script meant to last — types can be added. Always resolve via `sv options seogpt type --search <keyword>` (fuzzy) or require exact `--strict --no-fuzzy` in agent/script contexts. `--contenttype` is a CLI alias for `--type` — same field, either flag name works.

