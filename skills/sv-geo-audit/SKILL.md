---
name: sv-geo-audit
description: Run a GEO audit with the SV MCP `geogptaudit` tool (async): how a domain or page is visible in AI-generated answers for specific entities, with citation targets, schema fixes, quick wins and a roadmap. Use when the user asks for a GEO audit or AI-visibility score for named entities.
---

# SV GEO Audit: `geogptaudit`

**MCP tool:** `geogptaudit` · **Mode:** asynchronous (usually several minutes)
**API reference:** [GEO Audit](https://seovendor.co/api/geogptaudit.html)

## When to use this

Use for a deep audit of how a domain or page performs in generative engines such as ChatGPT, Gemini and Google AI Overviews, for entities the user already has in mind. It is deeper and slower than the technical check in `sv-preliminary-audit`. For open-ended questions like "what does AI associate with this site?" use the faster `sv-insight-igniter`.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `keyword` | yes | Target entities, comma separated, 1 to 5. Entities, not a single search phrase. |
| `url` | yes | A domain for a full-site audit, or a URL with a path for a single page. |
| `brand` | no | Defaults to the first entity. |
| `language` | no | 0 to 39. 0 = US English. |
| `scantype` | no | 0 = deep scan of all pages (default), 1 = quick scan of top pages. |
| `target_location` | no | Max 120 characters. |
| `wait` | no | See Async behavior below. |

## Example call

```json
{ "keyword": "white label seo, seo agency", "url": "https://example.com", "brand": "Acme SEO", "wait": true }
```

## Result

A finished result includes `ready`, the audited `url` and `brand`, and a set of lettered modules (A to K). They cover pages and entity mappings, schema fixes, consistency risks, citation targets and competitor patterns, quick wins, a roadmap and citation plan, intent gaps, profile checks, misrepresentation risks, and recommendation readiness and review sentiment.

## Chaining

Pair with `sv-ranklens` for the full picture: this tool audits the domain, RankLens measures brand visibility against competitors. If a gap needs done-for-you work, finish with `sv-marketplace-services`.

## Gotchas

- Not every module is guaranteed to be present in every report. Parse defensively.
- When summarizing for a person, lead with the quick wins and the roadmap, not the raw module dump.
- A deep scan of a large site takes longer. Use `scantype: 1` for a faster first pass.

## Async behavior

This tool returns a `task_id` immediately by default. To get the report either:

- call with `wait: true`. It waits up to 45 seconds. If the task is still running you get `status: still_running` and the `task_id`, or
- call `get_task_status` with the `task_id`, then `get_task_result` once it has finished.

Every call to `geogptaudit` starts a new, separately charged task. Never call it again to check progress or to retry a slow task. Use `get_task_status` and `get_task_result` instead, which are free. Pass `tool: "geogptaudit"` to them if the task was created in a different session.
