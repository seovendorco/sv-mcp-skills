# Claude Code

## Connect SV MCP

Add the server from your terminal:

```bash
claude mcp add --transport http sv-mcp https://mcp.seovendor.co
```

Then start Claude Code and run `/mcp`. SV MCP will show as needing authentication. Select it and follow the browser sign-in to SV. From the shell, `claude mcp login sv-mcp` runs the same flow.

To share the server with everyone on a project, add `--scope project`. That writes a `.mcp.json` file at the project root, which you can commit:

```json
{
  "mcpServers": {
    "sv-mcp": {
      "type": "http",
      "url": "https://mcp.seovendor.co"
    }
  }
}
```

Claude Code asks each person to approve a project-scoped server the first time they use it, and each person signs in to their own SV account.

## Install the skills

Each skill is a folder with a `SKILL.md`. Copy the folders into your project or your personal skills directory:

```bash
git clone https://github.com/seovendorco/sv-mcp-skills.git
cp -r sv-mcp-skills/skills/sv-* .claude/skills/          # this project only
# or: cp -r sv-mcp-skills/skills/sv-* ~/.claude/skills/   # all your projects
```

Claude Code picks them up automatically. Ask for the kind of task a skill covers, for example "run a GEO audit on example.com", or invoke one directly by name, such as `/sv-geo-audit`.

Sources: [Claude Code MCP docs](https://code.claude.com/docs/en/mcp), [Claude Code skills docs](https://code.claude.com/docs/en/skills). SV MCP reference: [seovendor.co/api/mcp](https://seovendor.co/api/mcp).
