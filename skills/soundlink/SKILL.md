---
name: soundlink
description: Analyze Soundlink music marketing attribution, cost per Spotify listener, campaign and creative performance, returning listeners, and catalog engagement. Also read soundlinks and wallet balance. All tools are read-only.
---

# Soundlink

Soundlink connects Meta ads to actual Spotify listening behavior. Help artists, managers, labels, and marketers understand which campaigns and creatives bring listeners, what those listeners cost, and whether they keep listening.

Use the connected Soundlink MCP tools for fresh evidence. One connection is one organization; ask the user to reconnect for another organization. Never invent data, URLs, campaign changes, or access to internal Soundlink agent tools.

## Choose the workflow

- **Campaign performance:** Resolve campaigns with `get_campaigns` and, when needed, `get_campaign`. Read `get_campaign_metrics_overview` for each relevant campaign using the same explicit date window. Lead with attributed listeners and returned cost per listener (CPL); add spend, followers and streams per listener where available. Keep each campaign's deduplicated totals separate.
- **Creative comparison:** Use `get_campaign_metrics_creatives` for lifetime performance grouped by background video and its variants. Pass no dates. Compare returned CPL alongside listener volume; distinguish missing values from zero. The first row is not automatically a proven winner. Spend is omitted from this report; do not invent it. If the user asks for a dated creative CPL comparison, explain that it is unavailable. `get_campaign_metrics_creative_daily` supports dated media spend, impressions, followers and cost per follower, not that CPL comparison.
- **Countries and returning listeners:** Use `get_campaign_metrics_breakdown` for daily country metrics, including new and returning listeners. Explain changes using the returned rows and their date grain; use overview for whole-window unique totals. Clicks and CTR can help diagnose delivery but do not establish which ads bring listeners most efficiently.
- **Catalog or playlist follow-through:** Use `get_campaign_metrics_engagement` to see what connected listeners play while the campaign is active. Choose `catalog` for same-artist listening or `playlist` for the promoted playlist; omit `engagementContext` when both are relevant. Compare the returned tracks with the promoted track before describing catalog spillover.
- **Standalone soundlinks:** Use `get_soundlinks`, `get_soundlink`, and corresponding soundlink metrics tools. The catalog excludes campaign-created soundlinks. Soundlink breakdown rows have no spend; do not manufacture a CPL from clicks.
- **Wallet or setup:** Use `get_wallet` for balance and campaign/tier/strategy tools for supported configuration details. Wallet balance is not campaign media spend. `get_campaign_tiers` only succeeds for wallet campaigns.

## Interpret metrics correctly

- Soundlink's attributed listeners are the basis for CPL. Dividing ad spend by an artist's aggregate Spotify for Artists listeners mixes campaign and other listening and does not measure campaign CPL.
- Prefer server-computed ratios and totals. Overview `listeners` means attributed new listeners; `total_listeners` includes distinct new and returning listeners in the window. `cpl` uses `listeners`; `streams_per_listener` uses `total_listeners`.
- Do not sum daily `listeners` or `total_listeners` into a unique audience, or sum campaign audiences into unique organization reach. Do not average daily cost ratios. For creative-daily CPF across days, use summed media spend divided by summed followers, only when the denominator is positive and all requested rows are available.
- Preserve currency and units supplied by the response. Distinguish `spend_media` from `spend_total` and fees; compare costs on the same basis.
- Returning-listener counts describe repeat listening, not a cohort retention rate. Do not claim a cohort report unless an available tool explicitly supplies it. The public API has retention endpoints, but the current MCP tool set does not expose them.
- Campaign listening history is synced while the campaign is active. After it ends, listeners connected only through that campaign stop syncing; a subsequent drop in observed engagement is not evidence of churn.

## Dates, completeness, and access

Resolve relative dates explicitly and state the window. For a weekly review, use the last seven complete days. Omitted overview dates mean campaign start or soundlink creation through today; creative-daily omitted dates mean every day with rows. Lifetime creatives never accept dates.

Engagement reads and JSONL exports default to the last 90 days and reject wider ranges. Follow pagination before presenting a complete comparison. Exports return JSONL and breakdown export rows have no spend. Recent data can be revised for seven days; qualify conclusions when that matters.

| Data | Scope |
| --- | --- |
| Campaigns, campaign activity, tiers, strategies | `campaigns:read` |
| Campaign metrics and exports | `metrics:read` |
| Soundlinks and their metrics and exports | `soundlinks:read` |
| Wallet | `wallet:read` |

If authentication fails, use `ping` to check the connection; a valid credential returns `{"status":"ok"}`. Ask the user to reconnect for an expired credential or missing consent. Do not substitute another organization's data.

## Deliver the answer

Start with the finding, show the relevant figures and window, then give a practical next step. Use campaign names and links actually returned by tools. Separate observed results from recommendations; identify low-volume or incomplete comparisons when material. Do not invent benchmarks, percentiles, or a statistical winner. Recommend only actions supported by Soundlink, such as reviewing a creative or campaign budget in the dashboard. This plugin cannot create or pause campaigns, change budgets, import videos, or make purchases.
