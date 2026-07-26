# RankLens — `ranklens_rank`

**MCP tool name:** `ranklens_rank`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/ranklens/` (action: `rank / competitors`)
**sv-cli command:** `sv ranklens` (aliases: (none))
**API reference:** [RankLens](https://seovendor.co/api/ranklens.html)
**Type:** Synchronous

## What it does

Measure AI brand visibility across 27 generative engines — how often and how prominently a brand surfaces in generative-engine answers — and benchmark against competitors.

## When to use this

Use when the question is specifically about brand-level AI visibility, and (via the `competitors` action) relative to named competitors.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `web` | string | required | Website URL, max 300 characters. |
| `url` | string | optional | Fallback website URL used when `web` is not sent. |
| `entity` | string | required | **The primary search term field** — brand, product, or topic, 4-80 characters. `kw` and `keyword` are accepted as fallback aliases if `entity` isn't sent, but response rows always return `entity` (never `keyword`). |
| `kw` | string | optional | Fallback for `entity`. |
| `keyword` | string | optional | Fallback for `entity` and `kw`, in that order. |
| `mgptid` | string | conditional | Encrypted MGPTID. **Required for the `competitors` action** (not used for `rank`) — obtained from a prior `rank` call's response (`data[].mgptid`). No CLI flag maps to this field; use a raw call. |
| `MGPTID` | string | optional | Fallback for `mgptid`. |
| `brand` | string | optional | Brand text, max 60 characters. |
| `location` | string | optional | Location text, max 100 characters. |
| `lang` | integer | optional | Language ID, same 40-language set. |
| `engine` | integer | optional | ~27 engine options: OpenAI ChatGPT variants, Google Gemini variants, Anthropic Claude Sonnet 4.5/4.6, xAI Grok variants, Deepseek, Llama 4 Maverick, and "(Latest)" aliases per family. |
| `samplesize` | integer | optional | 0=Default, 1-4 = 2x/4x/8x/16x MGPT sampling — higher values trade cost for statistical confidence. |
| `brandmatch` | integer | optional | ~180 options describing the comparison angle (Most Relevant, Best Value, Most Reliable, GDPR-Compliant, Made in USA, etc). Confirm via `sv options ranklens brandmatch --search <term>` rather than guessing. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options ranklens <field>` or `sv options ranklens <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show ranklens` shows the full current schema.

## Example request

```json
{
  "action": "rank",
  "web": "https://example.com",
  "entity": "white label seo",
  "brand": "Acme SEO"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

Array of ranking objects: `entity`, `domain`, `brand`, `lowrank`/`avgrank`/`highrank`, `confidence`, `appearance`, `iterations`, `probability`, `ci_high`/`ci_low`/`ci_width`, `brandmatch`, normalized variants (`norm_probability`, `norm_ciwidth`, `norm_confidence`), `rankscore`, and `mgptid` (needed for a follow-up `competitors` call). Same shape for both `rank` and `competitors` actions.

## Chaining with other tools

Commonly paired with `geo_audit_create_task` (domain-level GEO audit) for a full before/after or competitive picture. Two-step flow for competitor data: `rank` → grab `mgptid` from the response → `competitors` with that `mgptid`.

## Gotchas

This tool's field-naming is the second-biggest trap in the API (after seogpt2's `Topic`): send `entity`, not `kw`/`keyword`, even though both are accepted as fallbacks — and expect `entity` back in the response either way. The `competitors` action requires `mgptid` from a prior `rank` response and has no CLI flag for it — must be a raw call.

