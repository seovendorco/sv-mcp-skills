# Claude Desktop

## Recommended: add it as a connector

Claude Desktop uses the same connector flow as Claude.ai:

1. Open **Customize → Connectors**.
2. Click **+ → Add custom connector**.
3. Paste `https://mcp.seovendor.co` and click **Add**.
4. Click **Connect** and sign in to SV to approve access.

You need a free SV account first: [sign up](https://access.seovendor.co/signup).

## Alternative: run SV MCP locally with your API key

If you would rather run the server on your own machine, add it to Claude Desktop's config file and restart the app.

Config file location:

- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "sv-mcp": {
      "command": "uvx",
      "args": ["sv-mcp"],
      "env": { "SV_API_KEY": "your-key-here" }
    }
  }
}
```

This needs [uv](https://docs.astral.sh/uv/) installed. The `env` block belongs to a locally launched server like this one. It does nothing for the hosted `https://mcp.seovendor.co` URL, which signs you in with OAuth instead.

## Skills

The skills in [`skills/`](../skills) are written for Claude Code's skill format. In Claude Desktop, connect SV MCP first and use the skill files as reference or paste a skill into a conversation when you want the agent to follow it.

Full reference: [seovendor.co/api/mcp](https://seovendor.co/api/mcp)
