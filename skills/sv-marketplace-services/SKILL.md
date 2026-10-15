---
name: sv-marketplace-services
description: Search SV's catalog of purchasable done-for-you services (SEO, PPC, development) by search term with price, series and category filters, using the SV MCP `marketplace-services` tool. Use only when the user asks to find SV services.
---

# SV Marketplace Services: `marketplace-services`

**MCP tool:** `marketplace-services` · **Mode:** synchronous
**API reference:** [Marketplace Services](https://seovendor.co/api/marketplace-services.html)

## When to use this

Use only when the user wants a fulfillable, done-for-you service from SV instead of a self-serve tool run. It is often the last step after a diagnostic (`sv-preliminary-audit`, `sv-geo-audit`) turns up a gap the user wants handled for them.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `keyword` | yes | Search term, 1 to 200 characters. |
| `category` | no | `SEO`, `PPC` or `DEV`. |
| `series` | no | `i-SERIES`, `S-SERIES`, `SC-SERIES`, `X-SERIES`, `XC-SERIES`, `M-SERIES`, `AX-SERIES` or `FUNNEL`. |
| `price` | no | Price ceiling. Only services at or below it are returned. Omit or use 0 for no limit. |

## Example call

```json
{ "keyword": "technical seo audit", "category": "SEO", "price": "500" }
```

## Result

Up to 5 matching services, each with its category, series, name, price, subscription price, a description and a link.

## Gotchas

- Results are capped at 5. If the first search is too broad, narrow `keyword`, `series` or `category`. There is no pagination.
- The results are recommendations for a person to review. Do not purchase or commit to anything on the user's behalf.
