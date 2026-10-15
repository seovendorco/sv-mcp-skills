---
name: sv-topical-authority
description: Build a topical content plan for a keyword (suggested article topics, plus related subtopics in SEO mode) with the SV MCP `topical-authority` tool. Use for content strategy and planning once keyword research is done. To write the articles use sv-prose.
---

# SV Topical Authority: `topical-authority`

**MCP tool:** `topical-authority` · **Mode:** synchronous
**API reference:** [Topical Authority](https://seovendor.co/api/topical-authority.html)

## When to use this

Use once keyword research is done and it is time to plan a cluster of interlinked articles that build authority on a subject, rather than a single one-off piece. To write the articles themselves use `sv-prose`.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `keyword` | yes | 4 to 300 characters. |
| `topicmode` | no | `seo` (find SEO topics) or `geo` (find GEO topics). |
| `topicsize` | no | How many topics: 0 = 10, 1 = 20, 2 = 30. |
| `web` | no | Website URL, max 300 characters. `url` is a fallback. |
| `brand` | no | Max 60 characters. |
| `brandpersona` | no | Max 2000 characters. |
| `language` | no | 0 to 39. 0 = US English. |

## Example call

```json
{ "keyword": "white label seo", "topicmode": "seo", "topicsize": 1, "web": "https://example.com", "brand": "Acme SEO" }
```

## Result

A list of suggested article topics. In `seo` mode it also includes related subtopic ideas. The fields differ between `seo` and `geo` modes, so parse defensively.

## Chaining

`sv-better-keywords` → `sv-topical-authority` → `sv-prose` (once per planned topic) is the standard content-cluster pipeline. Follow with `sv-seo-mapping` to turn the plan into a page map.

## Gotchas

- The output is a plan, not content. Each topic still needs its own `sv-prose` call, and each of those is a separately charged task.
- Do not assume the same fields in `geo` mode as in `seo` mode.
