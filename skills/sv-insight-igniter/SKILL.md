---
name: sv-insight-igniter
description: Show which entities and topics AI engines associate with a website, given only its URL, using the SV MCP `insight-igniter` tool (fast, synchronous). Use for open-ended questions like what AI associates with a site. For specific entities use sv-geo-audit.
---

# SV Insight Igniter: `insight-igniter`

**MCP tool:** `insight-igniter` · **Mode:** synchronous and fast
**API reference:** [Insight Igniter](https://seovendor.co/api/insight-igniter.html)

## When to use this

Use to understand what entities and topics AI engines already link to a site, before writing competing or complementary content, or before a `sv-topical-authority` pass so you do not duplicate entity coverage. To score visibility for specific entities the user already has, use `sv-geo-audit` (slower, asynchronous).

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `web` | yes | Website URL, max 300 characters. `url` is accepted as a fallback. |
| `brand` | no | Max 60 characters. |
| `language` | no | 0 to 39. 0 = US English. |

## Example call

```json
{ "web": "https://example.com", "brand": "Acme SEO" }
```

## Result

One result set per AI engine queried. Each entity has a category, confidence, intent and demand value. Engines that failed are listed separately with an error code and message.

## Chaining

Often run on competitor URLs from `sv-top-competitors`, feeding `sv-topical-authority` or `sv-core-analysis`.

## Gotchas

- It queries several engines per call and can partially fail. Check the errors even when the call succeeds, since some engines may have returned nothing.
