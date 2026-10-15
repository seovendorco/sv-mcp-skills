---
name: sv-prose
description: Write a full long-form SEO or GEO article, blog post, guide or landing page with the SV MCP `prose` tool (async). Use when the user wants a complete article on a topic. For short snippets such as meta titles use sv-seogpt.
---

# SV Prose: `prose`

**MCP tool:** `prose` · **Mode:** asynchronous (usually several minutes)
**API reference:** [Prose (formerly SEO GPT 2)](https://seovendor.co/api/seogpt2.html)

## When to use this

Use for full-length blog posts, guides, case studies, white papers, landing pages and similar. For short snippets use `sv-seogpt`.

## Parameters

| Parameter | Required | Notes |
|---|---|---|
| `topic` | yes | The article title or subject, 12 to 200 characters. |
| `keyword` | no | Supporting search keywords, up to 5, each 4 to 70 characters, 300 total. This is a separate field from `topic`. |
| `brand` | no | Max 60 characters. |
| `url` | no | Website URL, max 200 characters. |
| `notes` | no | Extra instructions, max 150 plain-text characters. |
| `type` | no | 0 = On-Page Blog Article, 1 = Off-Page Guest Article, 2 = Webpage Content, 3 = Landing Page, 4 = Product Review, 5 = Case Study, 6 = White Paper, 7 = How-To Guide, 8 = Tutorial Content, 9 = User Guide, 10 = Opinion Piece, 11 = E-Book, 12 = Press Release. |
| `length` | no | Word-count preset, 0 to 14 (for example 5 = 2000, 13 = 4000, 14 = 5000). |
| `language` | no | 0 to 39. 0 = US English. |
| `tone` | no | 0 to 25. |
| `person` | no | 0 = First, 1 = Second, 2 = Third. |
| `writer` | no | 0 to 5 (Professional Writer, Marketing Expert, and so on). |
| `engine` | no | Writing engine ID, 0 to 24. |
| `pathway` | no | 0 to 12. 6 = GEO (Generative Engine Optimization). Higher numbers add agentic variants. |
| `sourceposture` | no | 0 = Auto, 1 = Brand-Owned, 2 = Neutral Editorial, 3 = Comparative / Review. |
| `wait` | no | See Async behavior below. |

## Example call

```json
{ "topic": "White Label SEO for Agencies", "keyword": "white label seo, seo reseller", "url": "https://example.com", "brand": "Acme SEO", "wait": true }
```

## Result

A finished result includes the article as HTML (`article_html`), the settings that were used, and four quality scores from 0 to 100: visit-website intent, keyword alignment, brand alignment and notes alignment. Check that the result is ready before using it.

## Chaining

`sv-topical-authority` → `sv-prose` (once per planned topic) → `sv-content-quality` (score the draft) → `sv-core-analysis` (compare against competitors).

## Gotchas

- `topic` is the title. Putting the title in `keyword` leaves `topic` empty and the call fails. `keyword` only holds supporting search terms.
- `topic` must be at least 12 characters.
- `notes` is capped at 150 characters, so keep instructions short.
- The article comes back as HTML. Convert it before pasting into a plain-text field.
- Pick `type`, `engine` and `pathway` from the tool schema. If the user's request does not clearly match one, ask.

## Async behavior

This tool returns a `task_id` immediately by default. To get the article either:

- call with `wait: true`. It waits up to 45 seconds. If the task is still running you get `status: still_running` and the `task_id`, or
- call `get_task_status` with the `task_id`, then `get_task_result` once it has finished.

Every call to `prose` starts a new, separately charged task. Never call it again to check progress or to retry a slow task. Use `get_task_status` and `get_task_result` instead, which are free. Pass `tool: "prose"` to them if the task was created in a different session.
