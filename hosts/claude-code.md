# Claude Code

Add sv-mcp as a remote server from the terminal:

```bash
claude mcp add sv-mcp --transport http https://mcp.seovendor.co

# Verify it registered
claude mcp list
```

Or commit a `.claude/mcp.json` to your project so every collaborator gets it automatically:

```json
{
  "servers": [
    {
      "name": "sv-mcp",
      "url": "https://mcp.seovendor.co",
      "transport": "http"
    }
  ]
}
```

To make the skills in this repo available to Claude Code, copy the relevant files from [`/skills`](../skills) into your project's `.claude/skills/` directory (or reference this repo directly).

Full reference: [seovendor.co/api/mcp](https://seovendor.co/api/mcp)
