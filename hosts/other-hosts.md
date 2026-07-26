# Other MCP Hosts

Any MCP-compatible host that supports HTTP transport can connect to sv-mcp using the same base URL: `https://mcp.seovendor.co`

| Host | Where to add | Value |
|---|---|---|
| Cursor | Settings → MCP Servers → Add Server → HTTP | `https://mcp.seovendor.co` |
| Windsurf | Settings → MCP → Add | `https://mcp.seovendor.co` |
| Cline / Roo | MCP Settings → Remote Server | `https://mcp.seovendor.co`, transport: http |
| Continue | `config.json` `mcpServers` array | url: `https://mcp.seovendor.co`, transport: `http` |
| Custom host | Any MCP 1.x SDK | HTTP transport, base URL `https://mcp.seovendor.co` |

The skill files in [`/skills`](../skills) are plain Markdown and aren't Claude-specific — they can be pointed to by any host that supports loading external instructions/skills alongside an MCP connection.

Full reference: [seovendor.co/api/mcp](https://seovendor.co/api/mcp)
