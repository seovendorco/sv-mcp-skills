---
name: sv-seogpt-compare
description: Get competitive SEO strategy for one URL and one target keyword with the SV MCP `seogptcompare` tool (async), which compares the page against its top competitors. Use when the question is strategic, such as what to prioritize for this page and keyword.
---

# SV SEO Strategist: `seogptcompare`

**MCP tool:** `seogptcompare` · **Mode:** asynchronous
**API reference:** [SEO Strategist](https://seovendor.co/api/seogptcompare.html)

## When to use this

Use when the ask is strategy-level ("what should our SEO priorities be for this page and keyword?") rather than a single technical or content task. It is a good first step on a new engagement. For just a list of competitors use `sv-top-competitors`.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `keyword` | yes | One target keyword, 4 to 69 characters. |
| `url` | yes | Target page URL, 4 to 300 characters. |
| `brand` | no | Brand name. |
| `country` | no | Country ID, 0 to 46. 0 = United States. |
| `language` | no | 0 to 39. 0 = US English. |
| `wait` | no | See Async behavior below. |

## Example call

```json
{ "keyword": "white label seo", "url": "https://example.com", "brand": "Acme SEO", "wait": true }
```

## Result

A finished result includes a written strategy plus detailed content analysis data for the page against its competitors. The result is keyed by an internal numeric ID rather than a fixed field name, so parse it defensively.

## Chaining

Run before keyword research or content production to set direction. Follow with `sv-better-keywords` and `sv-prose`.

## Gotchas

- `keyword` is capped at 69 characters, shorter than most other tools.
- It takes one keyword, not a list. For many keywords use `sv-seo-mapping`.

## Async behavior

This tool returns a `task_id` immediately by default. To get the analysis either:

- call with `wait: true`. It waits up to 45 seconds. If the task is still running you get `status: still_running` and the `task_id`, or
- call `get_task_status` with the `task_id`, then `get_task_result` once it has finished.

Every call to `seogptcompare` starts a new, separately charged task. Never call it again to check progress or to retry a slow task. Use `get_task_status` and `get_task_result` instead, which are free. Pass `tool: "seogptcompare"` to them if the task was created in a different session.
