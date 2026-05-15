# Vercel MCP

Vercel's official MCP server gives AI tools secure access to your Vercel projects.

**[Documentation](https://vercel.com/docs/mcp/vercel-mcp)** | **[Tools Reference](https://vercel.com/docs/mcp/vercel-mcp/tools)**

## Quick Start

Connect to `https://mcp.vercel.com` in your MCP client.

See the [documentation](https://vercel.com/docs/mcp/vercel-mcp) for setup instructions for Claude, ChatGPT, Cursor, VS Code, and other supported clients.

## Client setup

| Client | Guide |
|--------|--------|
| [Google Antigravity](https://antigravity.google/) | [docs/antigravity.md](docs/antigravity.md) |

### Google Antigravity (quick)

Antigravity uses `~/.gemini/antigravity/mcp_config.json` (Windows: `%USERPROFILE%\.gemini\antigravity\mcp_config.json`). Open it via **MCP Servers → Manage MCP Servers → View raw config**.

```json
{
  "mcpServers": {
    "vercel": {
      "serverUrl": "https://mcp.vercel.com",
      "headers": {
        "Authorization": "Bearer YOUR_VERCEL_TOKEN",
        "Content-Type": "application/json"
      }
    }
  }
}
```

Use `headers` for auth on remote MCP (not top-level `authorization`). Do **not** set `Accept` in `headers` — that commonly triggers **406 Not Acceptable**. Full steps and OAuth option: [docs/antigravity.md](docs/antigravity.md).
