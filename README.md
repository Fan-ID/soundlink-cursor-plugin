# Soundlink plugin

Connect Cursor to your Soundlink organization. The plugin is read-only. It calls `https://api.getsoundlink.com/mcp`.

Setup for an API key or an OAuth app: [MCP server](https://www.getsoundlink.com/docs/mcp).

## Install from this repo

Cursor loads a local plugin from `~/.cursor/plugins/local`. A symlink to a checkout somewhere else is ignored.

```bash
git clone https://github.com/Fan-ID/soundlink-cursor-plugin.git
mkdir -p ~/.cursor/plugins/local/soundlink
cp soundlink-cursor-plugin/plugin.json soundlink-cursor-plugin/mcp.json ~/.cursor/plugins/local/soundlink/
cp -R soundlink-cursor-plugin/skills ~/.cursor/plugins/local/soundlink/
```

Reload the window (**Developer: Reload Window**), then open **Customize**. You should see the Soundlink skill and the `soundlink` MCP server. Turn the server on.

On Enterprise, local plugins stay hidden until an admin enables **Allow Local Plugin Imports** under **Dashboard → Settings → Security & Identity → Marketplace and Plugins**.

## Sign in

Owners and admins create the credential. A connection that can read everything needs `campaigns:read`, `metrics:read`, `soundlinks:read`, and `wallet:read`.

**API key.** Create one in [Settings → Developer → API keys](https://getsoundlink.com/settings/developer). Add it only to the copy Cursor loads, never to this repo:

```json
{
  "$schema": "https://agent-plugins.org/schemas/1.0.0/mcp.schema.json",
  "mcpServers": {
    "soundlink": {
      "type": "streamable-http",
      "url": "https://api.getsoundlink.com/mcp",
      "headers": {
        "Authorization": "Bearer sk_your_key"
      }
    }
  }
}
```

Save that as `~/.cursor/plugins/local/soundlink/mcp.json` and reload.

**OAuth.** Soundlink does not support dynamic client registration. Create an app in [Settings → Developer → OAuth apps](https://getsoundlink.com/settings/oauth-apps) with the **Authorization code** grant, the redirect URI Cursor shows you, and the scopes above. When Cursor asks, paste that app's client id and secret. Access tokens last one hour. There are no refresh tokens.

## Check

Ask Cursor to call `ping`. A working credential returns `{"status":"ok"}`.
