# SEO GPT 2 — `seogpt2_create_task`

**MCP tool name:** `seogpt2_create_task`
**Underlying API endpoint:** `POST https://ai.seovendor.co/api/seogpt2/` (action: `createTask / getTaskStatus / getResult`)
**sv-cli command:** `sv seogpt2` (aliases: seo-gpt2)
**API reference:** [SEO GPT 2](https://seovendor.co/api/seogpt2.html)
**Type:** Async (createTask/getTaskStatus/getResult)

## What it does

Produce long-form SEO and GEO articles (500-5000 words) with control over content type, length, tone, voice, and pathway.

## When to use this

Use for full-length blog posts, pillar pages, guides, whitepapers, etc. — as opposed to `seogpt_generate`'s shorter, type-specific snippets.

## Request fields

Real field names and constraints, pulled directly from the live API schema (not guessed):

| Field | Type | Required | Description |
|---|---|---|---|
| `Topic` | string | required | **The article title/subject field.** 12-200 characters. CLI flag is `--topic` (alias `--title`), mapping directly to `Topic`. Confirmed current on the live SV CLI reference page as of 2026-07-26 — an earlier cached fetch of that same page showed the older `--keyword`-maps-to-`Topic` behavior, which the page now explicitly marks as superseded (`# WRONG (old behavior)`). |
| `KW` | string | optional | Separate, optional field for supporting search keywords — up to 5 keywords, 4-70 characters each, 300 total. CLI flag is `--keyword`/`--kw`/`--keywords` (all accepted) — do not confuse with `Topic`/`--topic`. |
| `Brand` | string | optional | Brand text, max 60 characters. |
| `URL` | string | optional | Website URL, max 200 characters. |
| `Notes` | string | optional | Notes/instructions, max 150 plain-text characters. |
| `ContentType` | integer | optional | 0-12: On-Page Blog Article, Off-Page Guest Article, Webpage Content, Landing Page, Product Review, Case Study, White Paper, How-To Guide, Tutorial Content, User Guide, Opinion Piece, E-Book, Press Release. |
| `ContentLength` | integer | optional | Word-count preset ID (200-5000 words). See `sv options seogpt2 lengths`. |
| `Language` | integer | optional | Language ID, same 40-language set as other tools. |
| `Tone` | integer | optional | 0-25: Professional, Informative, Engaging, Witty, Formal, Conversational, Persuasive, Humorous, etc. See `sv seogpt2 tones`. |
| `Person` | integer | optional | 0=First, 1=Second, 2=Third. |
| `Writer` | integer | optional | 0-5: Professional Writer, Marketing Expert, Freelance Story Writer, Business Blogger, Technical Writer, Seasoned Journalist. |
| `Engine` | integer | optional | 0-15 AI engine options, including Claude Sonnet 4.6, Claude Opus 4.7, GPT-5.6, Gemini 3.1 Pro, Grok 4.3, Deepseek v4 Pro. |
| `Pathway` | integer | optional | 0-6: Paragraphs, Page Segments, Press Format, Paragraphs (Reading Ease), Outline+Paragraphs, Outline+Paragraphs (Reading Ease), GEO. |
| `task_id` | string | conditional | Required for `getTaskStatus`/`getResult` — the encrypted ID returned by `createTask`. |

> Enum fields (`type`, `lang`, `engine`, etc.) are numeric IDs. **Never hard-code an ID from memory in a durable script or skill** — resolve it first. Via sv-cli: `sv options seogpt2 <field>` or `sv options seogpt2 <field> --search <term>`; add `--strict --no-fuzzy` in agent/script contexts to require an exact ID or slug. Via raw API: `sv definitions show seogpt2` shows the full current schema.

## Example request

```json
{
  "action": "createTask",
  "Topic": "White Label SEO for Agencies",
  "KW": "white label seo, seo reseller",
  "URL": "https://example.com",
  "Brand": "Acme SEO"
}
```

(`k` — your API key — is added automatically by sv-cli and sv-mcp; include it manually only for raw HTTP calls.)

## Response shape

`createTask`/`getTaskStatus`: `data.task_id`, `data.status` (pending/complete/error), `data.stage` (queued/processing/complete/error), `data.percent_complete`. `getResult`: adds `data.ready` (bool), `data.article_html`, `data.settings` (labels for the options used), and `data.scores` (0-100 alignment scores for visit_website/keyword/brand/notes intents).

## Chaining with other tools

`topical_authority_topics` → `seogpt2_create_task` → `content_quality_analyze` (score the draft) → `core_analysis_analyze` (check vs. competitors) is a common full research-to-scored-draft pipeline.

## Gotchas

**The single most important flag/field mapping in the whole API** — and it recently changed. `--topic` (alias `--title`) is now the CLI flag for the article title (`Topic`); `--keyword`/`--kw`/`--keywords` maps to the separate supporting-keywords field (`KW`). Older docs/scripts referencing `--keyword` as the title field are stale — the SV CLI reference page itself now explicitly flags that form as superseded. If a script or cached doc shows `sv seogpt2 create-task --keyword "..."` used alone as the title, treat it as out of date.

## Async behavior

This is one of four async SV tools (the others: `seogpt2_create_task`, `geo_audit_create_task`, `seogpt_compare_create_task`, `seo_mapping_create_task`). The underlying API action is `createTask`, which returns a `task_id` immediately (`status: pending`). Poll `getTaskStatus` with that `task_id` until `status` is `complete` (or `error`), then call `getResult` to fetch the finished payload — check `data.ready` (`true`/`false`) in the result.

Via sv-mcp: ask the agent to wait for the result and it polls internally rather than returning the bare task ID. Once wait mode triggers once in a session, later async calls in that session wait automatically too.

Via sv-cli directly: `--wait` (default timeout 600s, override with `--timeout`/`--poll-interval`), or a manual 3-step poll (`create-task` → `get-task-status` → `get-result`, passing `--tool TOOL_NAME` if the task wasn't created in the current CLI session).

