# Claude Desktop

Add sv-mcp to your Claude Desktop config file, then restart Claude Desktop.

**Config file location:**
- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Linux: `~/.config/claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "sv-mcp": {
      "url": "https://mcp.seovendor.co",
      "transport": "http"
    }
  }
}
```

Alternatively, bind your SV API key via an environment variable instead of the OAuth flow:

```json
{
  "mcpServers": {
    "sv-mcp": {
      "url": "https://mcp.seovendor.co",
      "transport": "http",
      "env": {
        "SV_API_KEY": "your-key-here"
      }
    }
  }
}
```

Once connected, copy the skill files from [`/skills`](../skills) into your project or reference them directly so Claude knows how and when to use each tool.

Full reference: [seovendor.co/api/mcp](https://seovendor.co/api/mcp)
