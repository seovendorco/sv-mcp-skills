# Cursor, VS Code, Windsurf and other hosts

Any MCP client that supports Streamable HTTP can connect to `https://mcp.seovendor.co`. Sign-in uses OAuth 2.1. You need a free SV account first: [sign up](https://access.seovendor.co/signup).

## Cursor

`~/.cursor/mcp.json`:

```json
{ "mcpServers": { "sv-mcp": { "url": "https://mcp.seovendor.co" } } }
```

## VS Code

`.vscode/mcp.json`:

```json
{ "servers": { "sv-mcp": { "type": "http", "url": "https://mcp.seovendor.co" } } }
```

## Windsurf

`~/.codeium/windsurf/mcp_config.json`:

```json
{ "mcpServers": { "sv-mcp": { "serverUrl": "https://mcp.seovendor.co" } } }
```

## Clients that only support stdio

Bridge to the hosted server with `mcp-remote`:

```json
{ "mcpServers": { "sv-mcp": { "command": "npx", "args": ["-y", "mcp-remote", "https://mcp.seovendor.co"] } } }
```

## Other hosts

For Cline, Roo, Continue or a custom client, use the host's remote or HTTP server option with the URL `https://mcp.seovendor.co`, or use the `mcp-remote` bridge above. Config formats differ between hosts and change often, so check your host's own MCP documentation for the exact file and field names.

## Skills

The files in [`skills/`](../skills) are plain Markdown with a short YAML header, so any host that can load external instructions can use them. They are not specific to Claude.

Full reference: [seovendor.co/api/mcp](https://seovendor.co/api/mcp)
