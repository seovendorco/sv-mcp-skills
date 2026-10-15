---
name: sv-seogpt
description: Generate short-form SEO or GEO copy (meta titles, meta descriptions, FAQs, social posts, ad copy) with the SV MCP `seogpt` tool. Use when the user wants one specific short piece of content from a keyword. For full articles use sv-prose.
---

# SV SEO GPT: `seogpt`

**MCP tool:** `seogpt` · **Mode:** synchronous (returns the text directly)
**API reference:** [SEO GPT](https://seovendor.co/api/seogpt.html)

## When to use this

Use for one specific, usually short piece of on-brand content: a meta title, a meta description, an FAQ answer, an ad headline, a social post. For a full article or blog post use `sv-prose`. To reshape text the user already has, use `sv-content-transformer`.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `keyword` | yes | 4 to 300 characters. |
| `type` | yes | Content type ID, 0 to 332. Examples: 0 = Title, 8 = Page Title, 18 = Meta Description, 26 = FAQ. |
| `web` | no | Website URL, max 300 characters. `url` is accepted as a fallback. |
| `brand` | no | Max 60 characters. |
| `brandpersona` | no | Max 2000 characters. |
| `length` | no | Length preset ID, 0 to 14. |
| `qty` | no | Number of variations, as an index: 0 = 1, 1 = 2, up to 9 = 10. |
| `language` | no | 0 to 39. 0 = US English. |
| `engine` | no | Writing engine ID, 0 to 18. |

The full ID lists are in the tool's own schema. Read them there instead of guessing.

## Example call

```json
{ "keyword": "white label seo", "type": 18, "web": "https://example.com", "brand": "Acme SEO" }
```

## Result

The generated text, as plain text or HTML depending on the content type. With `qty` above 0 you get several variations.

## Chaining

Usually the last step: `sv-better-keywords` → `sv-topical-authority` → `sv-seogpt` for the individual snippets in the plan.

## Gotchas

- There are 333 type IDs and new ones can be added. Never hard-code a `type` from memory. Look up the ID in the tool schema, and if the user's request does not clearly match one, ask which content type they want.
- `qty` is zero-based. `qty: 0` returns one item, not zero.
- `keyword` must be at least 4 characters.
