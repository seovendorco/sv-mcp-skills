---
name: sv-preliminary-audit
description: Run a quick technical SEO health check on a URL (canonical tags, blocking meta tags, load speed, SSL, robots, sitemap) and get an overall score with the SV MCP `preliminaryaudit` tool. Use as a fast first-pass diagnostic. For an element-by-element review use sv-core-analysis.
---

# SV Preliminary Audit: `preliminaryaudit`

**MCP tool:** `preliminaryaudit` · **Mode:** synchronous
**API reference:** [Preliminary Audit](https://seovendor.co/api/preliminaryaudit.html)

## When to use this

Use as a first technical check before deeper content or competitive work, to catch blockers early. For an element-by-element breakdown use `sv-core-analysis`. For AI-answer visibility use `sv-geo-audit`.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `url` | yes | The URL to audit. |

## Example call

```json
{ "url": "https://example.com" }
```

## Result

An overall score out of a maximum, plus named checks, each with its own score, a message and help text. They cover the URL and redirects, canonical tags, blocking meta tags, load speed, SSL (with and without www), URL consistency, robots.txt, crawlability and the sitemap.

## Chaining

A good starting point before `sv-core-analysis` or a content pipeline. It is lighter than `sv-geo-audit`.

## Gotchas

- Report the failing checks and their help text first. The overall score alone is not actionable.
