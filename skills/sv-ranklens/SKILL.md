---
name: sv-ranklens
description: Measure how a website ranks for an entity or keyword across repeated AI-engine queries, and find the competitors that appear, with the SV MCP `ranklens` tool. Use for brand-level AI visibility and competitor benchmarking.
---

# SV RankLens: `ranklens`

**MCP tool:** `ranklens` · **Mode:** synchronous
**API reference:** [RankLens](https://seovendor.co/api/ranklens.html)

## When to use this

Use when the question is about brand-level AI visibility, and (with the `competitors` action) how it compares to the competitors that show up in those AI answers. For a deep domain audit use `sv-geo-audit`.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `entity` | yes | The search term, 4 to 80 characters. Not `keyword`. |
| `web` | yes | Website URL, max 300 characters. `url` is accepted as a fallback. |
| `action` | no | `rank` (default) or `competitors`. |
| `mgptid` | for `competitors` | The `mgptid` from a prior `rank` result. Not used for `rank`. |
| `brand` | no | Max 60 characters. Pass it when the brand cannot be worked out from the domain. |
| `engine` | no | AI engine ID, 0 to 26. Several have a "Latest" alias. |
| `samplesize` | no | 0 = default, 1 to 4 = 2x, 4x, 8x, 16x sampling. More samples cost more and give more confidence. |
| `brandmatch` | no | Comparison angle, 0 to 182. |
| `location` | no | Max 100 characters. |
| `language` | no | 0 to 39. 0 = US English. |

## Example calls

Rank:

```json
{ "entity": "white label seo", "web": "https://example.com", "brand": "Acme SEO" }
```

Competitors, using the `mgptid` from the rank result:

```json
{ "action": "competitors", "entity": "white label seo", "web": "https://example.com", "mgptid": "<mgptid from the rank result>" }
```

## Result

`rank` returns one ranking data point per sample, including average, low and high rank, confidence, probability and a rank score, plus an `mgptid`. `competitors` returns the competitors that appeared in those samples. Rows use the field name `entity`.

## Chaining

Often paired with `sv-geo-audit`. The two-step competitor flow is `rank`, take `mgptid` from the result, then `competitors` with that `mgptid`.

## Gotchas

- The search term goes in `entity`, not `keyword`.
- `competitors` fails without a valid `mgptid` from an earlier `rank` call. No raw call is needed, it is an ordinary parameter.
- `brandmatch` has 183 options. If the user's request does not clearly match one, ask which they want instead of guessing an ID.
