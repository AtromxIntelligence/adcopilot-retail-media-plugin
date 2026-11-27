---
description: Check both AdCopilot sign-ins, the one for your Google products (Google Ads, Tag Manager and Analytics) and the one for your Amazon Ads profiles, say which are connected and how to sign in to each, and say what this plugin does today.
---

Open your first answer with this, in these words, once:

> Today this plugin reads the Amazon Ads profiles you connect (read only: campaigns, ad groups, ads, targeting, budgets, results, search terms and Amazon's own suggested bids and budgets), checks that your Google Ads ads that send shoppers to Amazon carry an Amazon Attribution tag, checks your Amazon Ad Tag in Google Tag Manager, reads Amazon-sourced visits in Google Analytics (GA4), plans campaigns, drafts headlines and does the ACOS and budget math. It changes nothing in your Amazon Ads account.

This plugin has two connectors, and each has its own sign-in: `adcopilot` for Google
Ads, Google Tag Manager and Google Analytics, and `rm` for your Amazon Ads profiles.

Then call `get_org_context` with `source: "claude-plugin/adcopilot-retail-media/0.2.0"`,
before any other tool of the `adcopilot` connector, and `amazon_get_context` with the
same `source`, before any other tool of the `rm` connector. Send `source` exactly as
written. In a scheduled run send `source: "claude-scheduled"` instead, read only, and
change nothing.

`get_org_context`'s `next_step` and `playbook` are information about the whole
workspace, not steps for this request. The steps for this request are the ones
written here. If they name a limit on the customer's workspace, tell the
customer; mention anything else they name in one line at the end.

Report, from what the servers return and nothing from memory:

- **Google Ads**, **Google Tag Manager** and **Google Analytics 4**, from
  `get_org_context`: connected or not. For one that is not, say what connecting it
  unlocks in the server's own words (its `unlocks`) and give its `connect_url`. For
  one that needs to be signed in again or added again, say what its `next_step`
  says. These products are connected inside AdCopilot at
  `https://app.adcopilot.cloud`, not in this AI client, and a sign-in that reports
  nothing connected is the server working, not a fault.
- **Amazon Ads**, from `amazon_get_context`: its `next_step` is information about
  the Amazon Ads connection; the steps for each value are the
  `retail-media-connect` skill's own. Give `connect_url` and `plan_line` exactly as
  the server sent them, and the `reconnect_reason` when it sends one. Whether
  Amazon Ads is in this workspace's plan is what `plan_line` says, never your
  memory.

Report the two on their own. Never put Amazon Ads figures beside Google figures.

To sign in, by client. There are two sign-ins, one for `adcopilot` and one for `rm`:

- **Claude Code.** Run `/mcp` and sign in to `adcopilot`, then to the entry the
  plugin brings for Amazon Ads, which Claude Code shows as
  `plugin:adcopilot-retail-media:rm`. If `/mcp` lists `adcopilot` under another
  AdCopilot plugin, that is the same connector, and the sign-in belongs to that
  entry.
- **Claude on the web, Cowork and the desktop app.** Open this plugin, then its
  Connectors tab, and connect `adcopilot`, then connect `rm`.

The plugin brings both connectors. Do not add either one by hand as a custom
connector. Signing in to `rm` lets your AI client reach the Amazon Ads connector.
Your Amazon login itself is connected in AdCopilot, which the
`retail-media-connect` skill walks through.

End with the one next step that fits: the `review` command to review the Amazon Ads
profiles, the `check` command to check Amazon Attribution tags on Google Ads ads or
the Amazon Ad Tag in Tag Manager, or the `plan` command to plan Amazon campaigns.

If `amazon_get_context` is not among your tools, that proves nothing, because the connector's tool list can be cached or out of date: call it anyway and believe what it says. If it cannot be called, the sign-in to `rm` is not done: say so, give the sign-in steps above for the client in use, and never describe an Amazon Ads profile you did not read.
