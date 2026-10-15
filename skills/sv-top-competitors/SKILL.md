---
name: sv-top-competitors
description: List the top-ranking competitor URLs for a keyword with the SV MCP `top-competitors` tool. Use early in a competitive analysis to decide who to benchmark against. For a full comparison of one URL against its competitors use sv-seogpt-compare.
---

# SV Top Competitors: `top-competitors`

**MCP tool:** `top-competitors` · **Mode:** synchronous
**API reference:** [Top Competitors](https://seovendor.co/api/top-competitors.html)

## When to use this

Use for a quick competitor list for a keyword, early in a competitive analysis. For a full comparison of a specific URL against its competitors use `sv-seogpt-compare`.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `keyword` | yes | 4 to 100 characters. |

## Example call

```json
{ "keyword": "white label seo" }
```

## Result

The top-ranking URLs for the keyword. Each has its link, its ranking position (1 is top) and a flag for whether it belongs to the queried site.

## Chaining

`sv-top-competitors` → `sv-insight-igniter` (per competitor) → `sv-core-analysis` is a common sequence for building a competitive brief.

## Gotchas

- There is no site or URL parameter. Whether a result is "your own site" is worked out by the server, not passed in.
