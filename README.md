# SV MCP Skills

Skills that teach AI agents (Claude, Claude Code, OpenClaw, Hermes Agent, Cursor, Windsurf and any other MCP-capable host) how to use [SV MCP](https://seovendor.co/api/mcp) well.

SV MCP is the hosted [Model Context Protocol](https://modelcontextprotocol.io) server for the [SV API](https://seovendor.co/api/). It exposes 15 SEO and GEO tools (keyword research, audits, AI-visibility scoring, competitor analysis, content generation) plus two task tools, `get_task_status` and `get_task_result`, for the long-running ones. The server lives at `https://mcp.seovendor.co`. Its source is in [sv-mcp](https://github.com/seovendorco/sv-mcp).

This repo is the skill layer, not the server. Each skill tells an agent when to reach for a tool, what to pass, what comes back, how to chain it with other tools, and what to watch out for.

## Why skills, not just tool descriptions

An MCP server exposes tool names and JSON schemas. That is enough for an agent to call a tool, but not to know when it should, what a good chain of tools looks like for a real task (for example "from a topic to a competitor-checked article"), or how to handle SV-specific behavior such as async tasks and points. These skills close that gap.

## Install

Each skill is a folder with a `SKILL.md` file, the format Claude Code loads.

**Claude Code.** Copy the skill folders into your project or your personal skills directory:

```bash
git clone https://github.com/seovendorco/sv-mcp-skills.git
cp -r sv-mcp-skills/skills/sv-* .claude/skills/        # this project only
# or: cp -r sv-mcp-skills/skills/sv-* ~/.claude/skills/   # all your projects
```

**Other hosts.** The files are plain Markdown with a short YAML header, so any host that can load external instructions can use them. Point it at the folders in [`skills/`](./skills).

The skills only help once SV MCP is connected. See [Connect SV MCP](#connect-sv-mcp).

## Connect SV MCP

You need a free SV account: [sign up for an API key](https://access.seovendor.co/signup). SV MCP signs you in with OAuth 2.1, so your API key stays on the server.

Setup per host is in [`hosts/`](./hosts):

- [Claude.ai](./hosts/claude-ai.md)
- [Claude Desktop](./hosts/claude-desktop.md)
- [Claude Code](./hosts/claude-code.md)
- [Cursor, VS Code, Windsurf and other hosts](./hosts/other-hosts.md)

## Skills

| Skill | MCP tool | What it does | Mode |
|---|---|---|---|
| [sv-seogpt](./skills/sv-seogpt/SKILL.md) | `seogpt` | Short-form SEO and GEO copy from a keyword: titles, meta descriptions, FAQs, posts | Sync |
| [sv-prose](./skills/sv-prose/SKILL.md) | `prose` | Full long-form articles | Async |
| [sv-content-transformer](./skills/sv-content-transformer/SKILL.md) | `content-transformer` | Rewrite existing text into a chosen content type | Sync |
| [sv-better-keywords](./skills/sv-better-keywords/SKILL.md) | `better-keywords` | Keyword research with volume, CPC and intent, or filter a list | Sync |
| [sv-insight-igniter](./skills/sv-insight-igniter/SKILL.md) | `insight-igniter` | Entities and topics AI engines associate with a site | Sync |
| [sv-topical-authority](./skills/sv-topical-authority/SKILL.md) | `topical-authority` | Topical content plan for a keyword | Sync |
| [sv-core-analysis](./skills/sv-core-analysis/SKILL.md) | `core-analysis` | On-page analysis of a URL against top-ranked pages | Sync |
| [sv-preliminary-audit](./skills/sv-preliminary-audit/SKILL.md) | `preliminaryaudit` | Quick technical SEO health score for a URL | Sync |
| [sv-geo-audit](./skills/sv-geo-audit/SKILL.md) | `geogptaudit` | GEO audit: visibility in AI-generated answers for given entities | Async |
| [sv-seogpt-compare](./skills/sv-seogpt-compare/SKILL.md) | `seogptcompare` | Compare a URL against its top competitors for a keyword | Async |
| [sv-seo-mapping](./skills/sv-seo-mapping/SKILL.md) | `seogptmapping` | Map keywords to pages on a domain, detect cannibalization | Async |
| [sv-ranklens](./skills/sv-ranklens/SKILL.md) | `ranklens` | AI brand visibility across repeated engine queries, and competitors | Sync |
| [sv-content-quality](./skills/sv-content-quality/SKILL.md) | `content-quality` | Score a live page's E-E-A-T and helpful-content quality | Sync |
| [sv-top-competitors](./skills/sv-top-competitors/SKILL.md) | `top-competitors` | Top-ranking competitor URLs for a keyword | Sync |
| [sv-marketplace-services](./skills/sv-marketplace-services/SKILL.md) | `marketplace-services` | Search SV's done-for-you services | Sync |

The image generator in the SV CLI (`seo-image`) is not exposed over MCP, so there is no skill for it.

## Conventions that apply to every skill

**Async tools.** `prose`, `geogptaudit`, `seogptcompare` and `seogptmapping` return a `task_id` immediately unless you pass `wait: true`, which waits up to 45 seconds. If the task is still running you get `still_running` and the `task_id`. Then call `get_task_status`, and `get_task_result` once it has finished. Pass `tool` to those two calls if the task was created in a different session.

**Cost.** Successful tool calls use points from the user's SV account. Failed calls, and calls to `get_task_status` and `get_task_result`, do not. Every call to an async tool starts a new, separately charged task, so never call one again just to check progress or retry a slow task.

**Rate limit.** The SV API accepts 1 request per second per key. SV MCP retries automatically.

**Enum IDs.** Many parameters (`type`, `language`, `engine`, `length`, `tone` and others) are integer IDs defined in each tool's schema. Read the IDs there. Never hard-code one from memory, since new options are added over time. If the user's request does not clearly match an option, ask which one they want.

**Parameter names.** These skills use the MCP parameter names (lowercase `keyword`, `url`, `language`, `topic`). The REST API and CLI use different names for some of the same fields, so do not copy field names from those docs into an MCP call.

## Chaining patterns

- **Research to scored draft:** `sv-better-keywords` → `sv-topical-authority` → `sv-prose` → `sv-content-quality` → `sv-core-analysis`
- **Competitive brief:** `sv-top-competitors` → `sv-insight-igniter` (per competitor) → `sv-seogpt-compare`
- **AI visibility picture:** `sv-geo-audit` + `sv-ranklens`, then `sv-marketplace-services` if the user wants a gap fixed for them
- **Site structure:** `sv-topical-authority` → `sv-seo-mapping`

The individual skill files list the chains each tool takes part in.

## Contributing

Corrections and additions are welcome. Open an issue or a pull request. If a parameter or tool name is out of date, include the source (a tool listing from a connected client, or the [SV API docs](https://seovendor.co/api/)).

## License

[MIT](./LICENSE)

---

Built by [SEO Vendor](https://seovendor.co), a white-label SEO and GEO platform for agencies. Related repos: [sv-mcp](https://github.com/seovendorco/sv-mcp) and [sv-cli](https://github.com/seovendorco/sv-cli).
