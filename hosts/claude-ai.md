# Claude.ai

Claude.ai's custom connector UI only needs the base URL — FastMCP mounts the MCP protocol endpoint there, separate from the OAuth callback/discovery routes also served on the same domain.

1. Go to **Settings → Integrations → Add custom integration**
2. Connector URL: `https://mcp.seovendor.co`
3. Claude handles the OAuth 2.1 flow automatically — your SV API key is bound during the OAuth handshake, not passed as a parameter on every call.

Full reference: [seovendor.co/api/mcp](https://seovendor.co/api/mcp)
