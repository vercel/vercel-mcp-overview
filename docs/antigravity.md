# Google Antigravity

[Google Antigravity](https://antigravity.google/) connects to Vercel MCP over Streamable HTTP. Use the native `serverUrl` configuration (not `command` / stdio).

## Configure

1. Open the Antigravity agent panel.
2. Click the **"..."** menu → **MCP Servers** → **Manage MCP Servers** → **View raw config**.
3. Edit `mcp_config.json`:
   - **macOS / Linux:** `~/.gemini/antigravity/mcp_config.json`
   - **Windows:** `%USERPROFILE%\.gemini\antigravity\mcp_config.json`
4. Add or merge the `vercel` entry below.
5. Save the file (Antigravity reloads MCP config automatically).

### OAuth (recommended)

If your Antigravity build supports OAuth for remote MCP, use only the server URL and complete sign-in when prompted:

```json
{
  "mcpServers": {
    "vercel": {
      "serverUrl": "https://mcp.vercel.com"
    }
  }
}
```

### Bearer token (manual)

When using a Vercel access token, put auth in `headers` (remote servers do not use the top-level `authorization` field):

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

## Important

- Use `serverUrl`, not `url`.
- Use `headers.Authorization` for remote MCP, not `authorization`.
- **Do not** set `Accept` in `headers`. Antigravity sends its own `Accept` value; overriding it often causes **406 Not Acceptable** during the Streamable HTTP handshake.
- If OAuth fails, try the [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) bridge (requires Node.js):

```json
{
  "mcpServers": {
    "vercel": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.vercel.com"]
    }
  }
}
```

## Verify

1. Open **MCP Servers** in the agent panel.
2. Confirm the Vercel server shows connected (not 406 / Not Acceptable).
3. Try a prompt such as “List my Vercel projects” or “Search Vercel docs for MCP”.

## Troubleshooting

| Symptom | Likely cause | Fix |
|--------|----------------|-----|
| **406 Not Acceptable** | `Accept` header mismatch (often from setting `Accept` in `headers`) | Remove custom `Accept`; keep only `Content-Type` and `Authorization` in `headers` |
| **401 Unauthorized** | Missing or invalid token | Refresh token; ensure `Bearer ` prefix |
| Schema error on `authorization` | Remote MCP must use `headers` | Move token to `headers.Authorization` |
| Server missing after edit | Invalid JSON | Validate `mcp_config.json` syntax |

For server-side negotiation issues, see <https://github.com/vercel/vercel-mcp-overview/issues/1>.
