# SV MCP Skills

Detailed, tool-by-tool skills that teach AI agents — Claude, OpenClaw, Hermes Agent, Cursor, Windsurf, Cline/Roo, Continue, and any other MCP-capable host — how to use [**SV MCP**](https://seovendor.co/api/mcp) effectively.

SV MCP is a [FastMCP](https://github.com/jlowin/fastmcp) server exposing all 16 tools on the [SV API](https://seovendor.co/api/) — SEO and GEO content generation, keyword research, entity extraction, technical audits, AI-visibility scoring, and more — natively to AI agents over MCP. It's built directly on the [sv-cli](https://github.com/seovendorco/sv-cli) core library, so behavior is identical whether a tool is called from the terminal, the REST API, or an MCP host.

This repo is **not** the MCP server itself — that lives at `https://mcp.seovendor.co` and is documented at [seovendor.co/api/mcp](https://seovendor.co/api/mcp). This repo is the **skill layer**: instructions that tell an agent *when* to reach for a given SV MCP tool, *what* it returns, *how* to chain it with other tools, and *what to watch out for* (async task polling, enum values, rate limits).

## Why skills, not just tool descriptions

An MCP server exposes tool names and JSON schemas — enough for an agent to technically call a tool, but not enough to know when it *should*, what a good chain of tools looks like for a real task ("go from a topic to a published, competitor-checked article"), or how to handle SV-specific behavior like async task polling. These skill files close that gap.

## Requirements

- An SV API key ([free signup](https://access.seovendor.co/signup))
- An MCP-capable host — see [`/hosts`](./hosts) for setup instructions specific to Claude Desktop, Claude Code, Claude.ai, Cursor, Windsurf, Cline/Roo, and Continue
- sv-mcp connected via `https://mcp.seovendor.co` (OAuth 2.1 — no manual API key handling required once connected)

## Hosts

Setup instructions per MCP host, in [`/hosts`](./hosts):

- [Claude.ai](./hosts/claude-ai.md)
- [Claude Desktop](./hosts/claude-desktop.md)
- [Claude Code](./hosts/claude-code.md)
- [Other hosts](./hosts/other-hosts.md) — Cursor, Windsurf, Cline/Roo, Continue, custom MCP 1.x hosts

## Note on a recently-changed field mapping

`seogpt2_create_task`'s CLI flag for the article title changed from `--keyword` to `--topic`/`--title` (confirmed against the live SV CLI reference page — a cache-busted refetch on 2026-07-26 showed the page now explicitly marks the old `--keyword`-only form as superseded). If you see older scripts, cached docs, or search results showing `sv seogpt2 create-task --keyword "..."` used alone as the title, treat it as stale. See [`skills/seogpt2-create-task.md`](./skills/seogpt2-create-task.md) for the current mapping.

## Cross-tool gotchas

The SV API has a few field-naming inconsistencies worth knowing before writing any automation, all called out in the relevant skill file too:

- **`seogpt2_create_task`**: the article title/topic field is `Topic` (capitalized). CLI flag is `--topic` (alias `--title`) — recently changed from an older `--keyword`-based mapping, now confirmed current on the live CLI docs. `--keyword`/`--kw`/`--keywords` maps to the separate supporting-keywords field (`KW`).
- **`ranklens_rank`**: the primary field is `entity` (CLI flag `--entity`), not `kw`/`keyword` (both still accepted as fallbacks, but responses always return `entity`). The `competitors` action additionally requires `mgptid` from a prior `rank` response — no CLI flag exists for it, so it needs a raw JSON call.
- **`better_keywords_research`**'s `filter` action requires a `data` array (max 1000 keyword objects) with no CLI flag — pass the array from a prior `research` call via a raw call.
- **Capitalized `URL`** (not `url`/`web`): `preliminary_audit_analyze`, `geo_audit_create_task`, and `seo_mapping_create_task` all require the field as `URL`.
- Several tools (`seogpt_generate`, `content_transformer_rewrite`) share the same 332-option `type` enum, and most tools share a 40-language `lang` enum — always resolve current IDs via the API's own definitions endpoint or the agent's tool discovery rather than hard-coding a number from these docs, since new options can be added over time.

## Skills

One skill file per SV MCP tool, in [`/skills`](./skills):

| Skill file | MCP tool | What it does |
|---|---|---|
| [seogpt-generate.md](./skills/seogpt-generate.md) | `seogpt_generate` | Generate SEO/GEO content across 300+ content types |
| [seogpt2-create-task.md](./skills/seogpt2-create-task.md) | `seogpt2_create_task` | Produce long-form SEO articles (async) |
| [content-transformer-rewrite.md](./skills/content-transformer-rewrite.md) | `content_transformer_rewrite` | Rewrite content to a new format, tone, or length |
| [seo-image-generate.md](./skills/seo-image-generate.md) | `seo_image_generate` | Generate SEO-optimized images from a keyword |
| [better-keywords-research.md](./skills/better-keywords-research.md) | `better_keywords_research` | Keyword research with volume, CPC, and intent |
| [insight-igniter-entities.md](./skills/insight-igniter-entities.md) | `insight_igniter_entities` | Extract SEO and GEO entities from a URL |
| [topical-authority-topics.md](./skills/topical-authority-topics.md) | `topical_authority_topics` | Generate topical authority article plans |
| [core-analysis-analyze.md](./skills/core-analysis-analyze.md) | `core_analysis_analyze` | Analyze a page against SEO competitors |
| [preliminary-audit-analyze.md](./skills/preliminary-audit-analyze.md) | `preliminary_audit_analyze` | Technical SEO and GEO health check |
| [geo-audit-create-task.md](./skills/geo-audit-create-task.md) | `geo_audit_create_task` | Audit generative engine visibility (async) |
| [seogpt-compare-create-task.md](./skills/seogpt-compare-create-task.md) | `seogpt_compare_create_task` | Strategic SEO guidance (async) |
| [seo-mapping-create-task.md](./skills/seo-mapping-create-task.md) | `seo_mapping_create_task` | Build an SEO map for a site (async) |
| [ranklens-rank.md](./skills/ranklens-rank.md) | `ranklens_rank` | AI brand visibility report vs. competitors |
| [content-quality-analyze.md](./skills/content-quality-analyze.md) | `content_quality_analyze` | Score HCU / E-E-A-T quality for a URL |
| [top-competitors-analyze.md](./skills/top-competitors-analyze.md) | `top_competitors_analyze` | Return the top 10 ranking competitor URLs |
| [marketplace-services-search.md](./skills/marketplace-services-search.md) | `marketplace_services_search` | Search the SV marketplace for services and packages |

## Async tools

Four tools are async and return a task ID by default rather than blocking: `seogpt2_create_task`, `geo_audit_create_task`, `seogpt_compare_create_task`, `seo_mapping_create_task`. If you ask the agent to wait for the result, sv-mcp polls internally until the task completes. Once wait mode is triggered once in a session, subsequent async calls in that session also wait automatically. Each async skill file calls this out explicitly.

## Chaining patterns

Common multi-tool workflows are documented inline in the relevant skill files (e.g. `better_keywords_research` → `seogpt2_create_task` → `content_quality_analyze` → `core_analysis_analyze` for a full brief-to-scored-draft pipeline). See individual skill files for the specific chains each tool participates in.

## Contributing

Corrections and additions welcome — open an issue or PR. If you find a tool parameter that's out of date, please link the source (a call log, the OAuth-scoped tool schema, or the [SV API docs](https://seovendor.co/api/)) rather than a guess.

## License

MIT

---

Built by [SEO Vendor](https://seovendor.co) — a white-label SEO/GEO platform for agencies, serving ~50,000 platform users since 2004. Powered by [sv-cli](https://github.com/seovendorco/sv-cli) and [sv-mcp](https://seovendor.co/api/mcp).
#   s v - m c p - s k i l l s 
 
 
