# SEO Mapping — `seo_mapping_create_task`

**MCP tool name:** `seo_mapping_create_task`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/seogptmapping/` (action: `createTask / getTaskStatus / getResult`)
**sv-cli command:** `sv seo-mapping` (aliases: mapping)
**API reference:** [SEO Mapping](https://seovendor.co/api/seogptmapping.html)
**Type:** Async (createTask/getTaskStatus/getResult)

## What it does

Build an advanced SEO map for a site and set of keywords — content-type mapping plus keyword cannibalization detection.

## When to use this

Use for site-wide structural planning — mapping keywords to content types/pages and catching cannibalization — rather than a single page or article.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `URL` | string | required | Target domain URL. |
| `kw` | string | required | Keywords to map, comma- or newline-separated. |
| `scantype` | integer | optional | 0=Deep scan (all pages, default), 1=Quick scan (top pages only). |
| `lang` | integer | optional | Language ID, same 40-language set. |
| `brand` | string | optional | Brand name. |
| `task_id` | string | conditional | Required for `getTaskStatus`/`getResult`. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options seogptmapping <field>` or `sv options seogptmapping <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show seogptmapping` shows the full current schema.

## Example request

```json
{
  "action": "createTask",
  "URL": "https://example.com",
  "kw": "white label seo, seo agency",
  "brand": "Acme SEO"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

`createTask`/`getTaskStatus`: `task_id`, `status`, `stage`, `percent_complete`. `getResult`: adds `ready`, `url`, `brand`, `created_at`, and `result` — an object keyed by integer content-type index (each `{type, data}`), plus a fixed key `"5"` holding `kwcannibal` (detected keyword cannibalization entries).

## Chaining with other tools

Pairs well with `topical_authority_topics` (topic clusters) to turn a content plan into an actual site structure/URL map.

## Gotchas

Like SEO Strategist, the result is keyed by internal numeric/integer indices rather than fixed field names — parse defensively. Uses `URL` (capitalized) like GEO Audit and Preliminary Audit, not `url`/`web` like most content tools.

## Async behavior

This is one of four async SV tools (the others: `seogpt2_create_task`, `geo_audit_create_task`, `seogpt_compare_create_task`, `seo_mapping_create_task`). The underlying API action is `createTask`, which returns a `task_id` immediately (`status: pending`). Poll `getTaskStatus` with that `task_id` until `status` is `complete` (or `error`), then call `getResult` to fetch the finished payload — check `data.ready` (`true`/`false`) in the result.

Via sv-mcp: ask the agent to wait for the result and it polls internally rather than returning the bare task ID. Once wait mode triggers once in a session, later async calls in that session wait automatically too.

Via sv-cli directly: `--wait` (default timeout 600s, override with `--timeout`/`--poll-interval`), or a manual 3-step poll (`create-task` → `get-task-status` → `get-result`, passing `--tool TOOL_NAME` if the task wasn't created in the current CLI session).

