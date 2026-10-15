---
name: sv-better-keywords
description: Research keywords with volume, CPC, competition and search intent, or AI-filter an existing keyword list, with the SV MCP `better-keywords` tool. Use at the start of a content pipeline, or when the user asks for keyword ideas.
---

# SV Better Keywords: `better-keywords`

**MCP tool:** `better-keywords` · **Mode:** synchronous
**API reference:** [Better Keywords AI](https://seovendor.co/api/better-keywords.html)

## When to use this

Usually the first tool in a content pipeline: find and validate target keywords before planning topics or writing content. The `filter` action trims an existing list down to the terms that matter.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `keyword` | yes | Seed keyword, 4 to 300 characters. Required for both actions. |
| `action` | no | `research` (default) or `filter`. |
| `data` | for `filter` | An array of keyword objects, max 1000. Each needs `keyword`. Optional: `competition` (0 to 100), `volume`, `score`, `category`, `intent`, `cpc`. |
| `web` | no | Website URL, max 300 characters. When given, results show whether the site already ranks for each keyword. `url` is a fallback. |
| `brand` | no | Max 60 characters. |
| `brandpersona` | no | Max 2000 characters. |
| `location` | no | Max 50 characters. |
| `language` | no | 0 to 39. 0 = US English. |
| `researchtype` | no | Array of research-angle IDs, 0 to 65. |
| `kwcompetition` | no | Array of competition-profile IDs, 0 to 31. |

## Example calls

Research:

```json
{ "keyword": "white label seo", "web": "https://example.com", "brand": "Acme SEO" }
```

Filter, passing the rows from a prior research call:

```json
{ "action": "filter", "keyword": "white label seo", "data": [ { "keyword": "white label seo services", "volume": 1000 } ] }
```

## Result

`research` returns keyword objects with volume, CPC, competition (0 to 100), intent, a score, and the site's current rank if `web` was given, plus provenance fields. `filter` returns only the rows that pass the relevance filter.

## Chaining

`sv-better-keywords` → `sv-topical-authority` → `sv-prose` is the standard research-to-draft pipeline. Results also feed `sv-ranklens` and `sv-top-competitors`.

## Gotchas

- `filter` needs the `data` array. Pass the rows from a `research` call directly.
- The `filter` result uses different field names (capitalized `Keyword`, plus a `DTR` score) from the `research` result. Do not parse them the same way.
- Rows submitted to `filter` without a `keyword` field are dropped.
- Pick `researchtype` and `kwcompetition` IDs from the tool schema. If the request does not clearly match one, ask.
