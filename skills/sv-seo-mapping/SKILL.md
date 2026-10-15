---
name: sv-seo-mapping
description: Map a list of keywords to the most relevant existing pages on a domain, and detect keyword cannibalization, with the SV MCP `seogptmapping` tool (async). Use for site-wide structure, internal linking and content-gap planning.
---

# SV SEO Mapping: `seogptmapping`

**MCP tool:** `seogptmapping` · **Mode:** asynchronous
**API reference:** [SEO Mapping](https://seovendor.co/api/seogptmapping.html)

## When to use this

Use for site-wide structural planning: which existing page best fits each keyword, where content is missing, and where pages compete for the same keyword. For a single page or article use another tool.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `keyword` | yes | The keywords to map, separated by commas or newlines. |
| `url` | yes | The target domain URL. |
| `brand` | no | Brand name. |
| `language` | no | 0 to 39. 0 = US English. |
| `scantype` | no | 0 = deep scan of all pages (default), 1 = quick scan of top pages. |
| `wait` | no | See Async behavior below. |

## Example call

```json
{ "keyword": "white label seo, seo agency, seo reseller", "url": "https://example.com", "brand": "Acme SEO", "wait": true }
```

## Result

A finished result maps keywords to content types and pages, and includes a list of detected keyword cannibalization. The result is keyed by internal numeric indices rather than fixed field names, so parse it defensively.

## Chaining

Pairs well with `sv-topical-authority`: turn the topic cluster into an actual page and URL map.

## Gotchas

- Cannibalization results are the part most worth surfacing to a person. Put them first.
- A deep scan of a large domain takes longer. Use `scantype: 1` for a quick first pass.

## Async behavior

This tool returns a `task_id` immediately by default. To get the map either:

- call with `wait: true`. It waits up to 45 seconds. If the task is still running you get `status: still_running` and the `task_id`, or
- call `get_task_status` with the `task_id`, then `get_task_result` once it has finished.

Every call to `seogptmapping` starts a new, separately charged task. Never call it again to check progress or to retry a slow task. Use `get_task_status` and `get_task_result` instead, which are free. Pass `tool: "seogptmapping"` to them if the task was created in a different session.
