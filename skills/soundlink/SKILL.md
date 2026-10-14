---
name: soundlink
description: Use when the user asks about their Soundlink organization, campaigns, soundlinks, ad metrics, wallet balance, or campaign strategies. The Soundlink MCP tools are read-only.
---

# Soundlink

Use the Soundlink MCP tools. Do not invent campaign, budget, soundlink, or video changes. Those writes are not available.

Call `ping` when a tool fails before changing the question. A valid credential returns `{"status":"ok"}`.

## Scope

| Data                                                | Scope             |
| --------------------------------------------------- | ----------------- |
| Campaigns, one campaign, tiers, strategies          | `campaigns:read`  |
| Campaign metrics and campaign JSONL exports         | `metrics:read`    |
| Soundlinks and soundlink metrics, including exports | `soundlinks:read` |
| Wallet                                              | `wallet:read`     |

One connection is one organization. Ask the user to reconnect if they need another org.

## Gotchas

- `get_campaign_tiers` only succeeds for a wallet campaign.
- `get_soundlinks` does not include soundlinks created for a campaign.
- Omit dates on a campaign overview to get campaign start through today. Omit dates on a soundlink overview to get soundlink creation through today.
- Engagement reads and every JSONL export default to the last 90 days. A wider range is an error. Export tools return JSONL, not the JSON envelope. Breakdown export rows have no spend.
- Pass `engagementContext` as `catalog` or `playlist` only when the user wants one of those. Omit it to get both.
- Do not add `listeners` across days.
