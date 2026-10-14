# Soundlink plugin

Connect Cursor to your Soundlink organization. The plugin is read-only. It calls `https://api.getsoundlink.com/mcp`.

Create the key in [Settings → Developer → API keys](https://getsoundlink.com/settings/developer). To read everything the server exposes, include `campaigns:read`, `metrics:read`, `soundlinks:read`, and `wallet:read`. Field details: [MCP server](https://www.getsoundlink.com/docs/mcp-server).

## Install from this repo

Cursor loads a local plugin from `~/.cursor/plugins/local`. A symlink to a checkout somewhere else is ignored.

```bash
git clone https://github.com/Fan-ID/soundlink-cursor-plugin.git
rm -rf ~/.cursor/plugins/local/soundlink
mkdir -p ~/.cursor/plugins/local/soundlink/.cursor-plugin
cp soundlink-cursor-plugin/.cursor-plugin/plugin.json ~/.cursor/plugins/local/soundlink/.cursor-plugin/
cp soundlink-cursor-plugin/mcp.json ~/.cursor/plugins/local/soundlink/
cp -R soundlink-cursor-plugin/skills ~/.cursor/plugins/local/soundlink/
```

Reload the window (**Developer: Reload Window**), then open **Customize**. Turn on the `soundlink` server. Open **Configure** and paste the API key. Cursor stores it for you. Do not put the key in this repo.

On Enterprise, local plugins stay hidden until an admin enables **Allow Local Plugin Imports** under **Dashboard → Settings → Security & Identity → Marketplace and Plugins**.

## Check

Ask Cursor to call `ping`. A working credential returns `{"status":"ok"}`.
