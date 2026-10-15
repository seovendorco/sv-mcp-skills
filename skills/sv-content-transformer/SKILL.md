---
name: sv-content-transformer
description: Rewrite or reformat text the user already has into a chosen content type (meta title, product description, landing-page copy and more) with the SV MCP `content-transformer` tool. Use to reshape existing copy. To write new content use sv-seogpt or sv-prose.
---

# SV Content Transformer: `content-transformer`

**MCP tool:** `content-transformer` · **Mode:** synchronous
**API reference:** [Content Transformer](https://seovendor.co/api/content-transformer.html)

## When to use this

Use when there is already a piece of text (a draft, a client's existing copy) that needs condensing, retoning or reformatting. To write something new from a keyword use `sv-seogpt` (short) or `sv-prose` (long).

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `text` | yes | The source text, 11 to 2000 characters. |
| `type` | no | Target content type ID, 0 to 332. The same list as `sv-seogpt`. |
| `keyword` | no | 4 to 300 characters. Anchors the rewrite to a target term. |
| `brand` | no | Max 60 characters. |
| `brandpersona` | no | Max 2000 characters. |
| `web` | no | Website URL, max 300 characters. `url` is a fallback. |
| `length` | no | Length preset ID, 0 to 14. |
| `language` | no | 0 to 39. 0 = US English. |
| `engine` | no | Writing engine ID, 0 to 18. |

## Example call

```json
{ "text": "Paste the source text you want to transform here.", "type": 18, "keyword": "white label seo", "brand": "Acme SEO" }
```

## Result

The rewritten text.

## Chaining

Commonly follows `sv-prose` to repurpose a long article into shorter formats. If you use competitor copy as the source, rewrite it thoroughly. Do not pass it through near-verbatim.

## Gotchas

- `text` is required and capped at 2000 characters. There is no URL fetch, so the caller must supply the source text. Split longer content into chunks.
- Pick the `type` ID from the tool schema. If the request does not clearly match one, ask which content type the user wants.
