# Content Transformer — `content_transformer_rewrite`

**MCP tool name:** `content_transformer_rewrite`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/content-transformer/` (action: `rewrite`)
**sv-cli command:** `sv content-transformer` (aliases: transform)
**API reference:** [Content Transformer](https://seovendor.co/api/content-transformer.html)
**Type:** Synchronous

## What it does

Rewrite and convert content between formats, tones, and lengths while preserving meaning and brand voice.

## When to use this

Use when there's already a piece of content (a draft, a client's existing copy) that needs reshaping — condensing, retoning, or reformatting — rather than generating something new.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `content` | string | required | Source content text, 11-2000 characters. CLI flag is `--text` (not `--content`) per the quick sheet — maps directly to this field. |
| `kw` | string | optional | Keyword text, 4-300 characters when provided — anchors the rewrite to a target term. |
| `brand` | string | optional | Brand text, max 60 characters. |
| `web` | string | optional | Website URL, max 300 characters. |
| `url` | string | optional | Fallback website URL used when `web` is not sent. |
| `brandpersona` | string | optional | Brand persona text, max 2000 characters. |
| `type` | integer | optional | Target content-type ID — same 332-option enum as `seogpt_generate`. Confirm via `sv options content-transformer type --search <term>`. |
| `length` | integer | optional | Length preset ID, same enum as SEO GPT. |
| `lang` | integer | optional | Language ID, same 40-language set. |
| `engine` | integer | optional | AI engine ID, same 0-8 set as SEO GPT. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options content-transformer <field>` or `sv options content-transformer <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show content-transformer` shows the full current schema.

## Example request

```json
{
  "action": "rewrite",
  "content": "Paste the source content you want to transform here.",
  "web": "https://example.com",
  "brand": "Acme SEO"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

`data.text` — the rewritten content as a single string. Synchronous, HTTP 200.

## Chaining with other tools

Commonly follows `seogpt2_create_task` (repurpose a long-form article into shorter derivative formats) or wraps competitor content pulled via `insight_igniter_entities`. Reword thoroughly — don't launder competitor copy near-verbatim.

## Gotchas

`content` (11-2000 chars) is required — there is no URL-fetch option; the caller must supply the actual source text. `action` only supports `rewrite` — passing anything else errors with "Action must be rewrite".

