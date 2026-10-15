---
name: sv-core-analysis
description: Run an element-by-element on-page SEO analysis of a URL (status, load speed, title, meta description, headings, keyword usage) and compare it with the top-ranked pages, using the SV MCP `core-analysis` tool. Use to benchmark a page against competitors.
---

# SV CORE Analysis: `core-analysis`

**MCP tool:** `core-analysis` · **Mode:** synchronous
**API reference:** [CORE Analysis](https://seovendor.co/api/core-analysis.html)

## When to use this

Use after content is drafted or published, to benchmark it against the current top-ranked pages for a keyword. For a single overall health score use `sv-preliminary-audit` instead.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `url` | no | Website URL, max 300 characters. |
| `keyword` | no | Max 100 characters. |

Neither is marked required in the schema, but a useful analysis needs both.

## Example call

```json
{ "url": "https://example.com/white-label-seo", "keyword": "white label seo" }
```

## Result

A dense, nested report: HTTP status and load speed, URL and SSL checks, a keyword uniqueness score, the title, meta description and heading structure with counts and term frequency, indexing, sitemap and robots.txt status, and the top-ranked sites for the keyword.

## Chaining

Pairs with `sv-top-competitors` (who to benchmark against) and usually runs after `sv-prose` to check a fresh draft before publishing.

## Gotchas

- This is the deepest result in the set. When summarizing for a person, pull out the scores and the top-ranked sites that drive a decision, and do not dump the raw structure.
- An empty call returns an error or a near-empty report.
