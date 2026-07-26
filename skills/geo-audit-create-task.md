# GEO Audit — `geo_audit_create_task`

**MCP tool name:** `geo_audit_create_task`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/geogptaudit/` (action: `createTask / getTaskStatus / getResult`)
**sv-cli command:** `sv geo-audit` (aliases: geogpt-audit, audit)
**API reference:** [GEO Audit](https://seovendor.co/api/geogptaudit.html)
**Type:** Async (createTask/getTaskStatus/getResult)

## What it does

Audit how a URL/domain performs across generative engines and AI answers — a deep, modular GEO report (citation targets, schema remediation, content-prompt mapping, reputation signals, and more).

## When to use this

Use for a deep audit of brand/domain visibility inside AI-generated answers — distinct from and deeper than `preliminary_audit_analyze`'s technical check.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `URL` | string | required | Domain URL for a full-site audit, or a URL with path for a single-page audit. |
| `kw` | string | required | Target entities, comma-separated, 1-5 entries. |
| `target_location` | string | optional | Target geographic location, max 120 characters. |
| `scantype` | integer | optional | 0=Deep scan (all pages, default), 1=Quick scan (top pages only). |
| `lang` | integer | optional | Language ID, same 40-language set. |
| `brand` | string | optional | Brand name. Defaults to the first entity in `kw` when not provided. |
| `task_id` | string | conditional | Required for `getTaskStatus`/`getResult`. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options geogptaudit <field>` or `sv options geogptaudit <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show geogptaudit` shows the full current schema.

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

`createTask`/`getTaskStatus`: `task_id`, `status`, `stage`, `percent_complete`. `getResult`: adds `ready`, `url`, `brand`, `created_at`, and `result.modules` — lettered modules A-K covering pages/entity mappings/schema remediation (C), consistency risks (D), citation targets/competitor patterns (E), quick wins/roadmap/citation plan (F), intent gaps (G), artifact signals (H), profile checklist (I), misrepresentation/multimodal checks (J), and recommendation readiness/review sentiment (K).

## Chaining with other tools

Often paired with `ranklens_rank` for a full AI-visibility picture: `geo_audit_create_task` for the domain-level audit, `ranklens_rank` for competitor-relative brand visibility.

## Gotchas

`kw` here means target *entities* (comma-separated, max 5), not a single search keyword — different semantics from most other tools' `kw` field. The result's `modules` object is deeply structured per lettered module; don't assume every module key is always present in every report.

## Async behavior

This is one of four async SV tools (the others: `seogpt2_create_task`, `geo_audit_create_task`, `seogpt_compare_create_task`, `seo_mapping_create_task`). The underlying API action is `createTask`, which returns a `task_id` immediately (`status: pending`). Poll `getTaskStatus` with that `task_id` until `status` is `complete` (or `error`), then call `getResult` to fetch the finished payload — check `data.ready` (`true`/`false`) in the result.

Via sv-mcp: ask the agent to wait for the result and it polls internally rather than returning the bare task ID. Once wait mode triggers once in a session, later async calls in that session wait automatically too.

Via sv-cli directly: `--wait` (default timeout 600s, override with `--timeout`/`--poll-interval`), or a manual 3-step poll (`create-task` → `get-task-status` → `get-result`, passing `--tool TOOL_NAME` if the task wasn't created in the current CLI session).

