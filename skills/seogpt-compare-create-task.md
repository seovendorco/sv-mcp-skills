# SEO Strategist — `seogpt_compare_create_task`

**MCP tool name:** `seogpt_compare_create_task`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/seogptcompare/` (action: `createTask / getTaskStatus / getResult`)
**sv-cli command:** `sv seogpt-compare` (aliases: compare)
**API reference:** [SEO Strategist](https://seovendor.co/api/seogptcompare.html)
**Type:** Async (createTask/getTaskStatus/getResult)

## What it does

Get strategic SEO guidance for a URL and target keyword.

## When to use this

Use when the ask is strategy-level ('what should our SEO priorities be for this page/keyword') rather than a single-page technical or content task.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `url` | string | required | Target page URL, 4-300 characters. |
| `kw` | string | required | Single target keyword, 4-69 characters. |
| `lang` | integer | optional | Language ID, same 40-language set. |
| `country` | integer | optional | Country ID, ~46 options (0=United States, then Brazil, Canada, China, France, Germany, India, UK, etc). |
| `brand` | string | optional | Brand name. |
| `task_id` | string | conditional | Required for `getTaskStatus`/`getResult`. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options seogptcompare <field>` or `sv options seogptcompare <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show seogptcompare` shows the full current schema.

## Example request

```json
{
  "action": "createTask",
  "url": "https://example.com",
  "kw": "white label seo",
  "brand": "Acme SEO"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

`createTask`/`getTaskStatus`: `task_id`, `status`, `stage`, `percent_complete`. `getResult`: adds `ready`, `url`, `keyword`, `brand`, `created_at`, and `result` keyed by an internal numeric id (observed: `"5"`) containing `strategy` (text) and `contentdtr` (array of detailed content analysis data).

## Chaining with other tools

Good entry point for a new engagement — run before diving into keyword research or content production, to set direction.

## Gotchas

`kw` is capped at 69 characters — shorter than most other tools' keyword fields (which allow up to 300). The `result` object's top-level key is a numeric string rather than a fixed field name — parse defensively rather than assuming the key.

## Async behavior

This is one of four async SV tools (the others: `seogpt2_create_task`, `geo_audit_create_task`, `seogpt_compare_create_task`, `seo_mapping_create_task`). The underlying API action is `createTask`, which returns a `task_id` immediately (`status: pending`). Poll `getTaskStatus` with that `task_id` until `status` is `complete` (or `error`), then call `getResult` to fetch the finished payload — check `data.ready` (`true`/`false`) in the result.

Via sv-mcp: ask the agent to wait for the result and it polls internally rather than returning the bare task ID. Once wait mode triggers once in a session, later async calls in that session wait automatically too.

Via sv-cli directly: `--wait` (default timeout 600s, override with `--timeout`/`--poll-interval`), or a manual 3-step poll (`create-task` → `get-task-status` → `get-result`, passing `--tool TOOL_NAME` if the task wasn't created in the current CLI session).

