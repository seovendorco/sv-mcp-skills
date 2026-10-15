---
name: sv-content-quality
description: Score a live page's content quality for a target keyword (E-E-A-T and helpful-content criteria) with the SV MCP `content-quality` tool, optionally comparing two URLs. Use after drafting and publishing, or to evaluate an existing page.
---

# SV Content Quality: `content-quality`

**MCP tool:** `content-quality` · **Mode:** synchronous
**API reference:** [Content Quality](https://seovendor.co/api/content-quality.html)

## When to use this

Use to score a specific published page against Google's helpful-content and E-E-A-T signals, typically right after a draft goes live or on an existing page. Pass a second URL to compare two pages head to head.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `keyword` | yes | 4 to 300 characters. |
| `url` | yes | The page to evaluate, max 300 characters. |
| `url_b` | no | A second page to compare against, max 300 characters. |

## Example call

```json
{ "keyword": "white label seo", "url": "https://example.com/white-label-seo" }
```

## Result

Up to 15 scored criteria per URL (30 when `url_b` is set). Each has a number, a score from 0 to 100, and a written reason.

## Chaining

`sv-prose` → publish → `sv-content-quality` → revise if scores are weak → `sv-core-analysis` is a solid quality gate.

## Gotchas

- It needs a live, public URL. There is no way to pass raw text, so a draft has to be published (or hosted somewhere reachable) first.
- Treat the scores as a prompt to read each `reason`, not as one pass or fail number.
- `keyword` must be at least 4 characters.
