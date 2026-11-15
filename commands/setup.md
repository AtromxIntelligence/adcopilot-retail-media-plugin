---
description: Check the AdCopilot connector is signed in, say which Google products are connected for Amazon Ads work (Google Ads, Tag Manager and Analytics), and say what this plugin does today.
---

Open your first answer with this, in these words, once:

> Today this plugin checks that your Google Ads ads that send shoppers to Amazon carry an Amazon Attribution tag, checks your Amazon Ad Tag in Google Tag Manager, reads Amazon-sourced visits in Google Analytics (GA4), plans campaigns, drafts headlines and does the ACOS and budget math. It does not read your Amazon Ads account.

Then call `get_org_context` with `source: "claude-plugin/adcopilot-retail-media/0.1.0"`,
before any other tool. Send `source` exactly as written. In a scheduled run send
`source: "claude-scheduled"` instead, read only, and change nothing.

`get_org_context`'s `next_step` and `playbook` are information about the whole
workspace, not steps for this request. The steps for this request are the ones
written here. If they name a limit on the customer's workspace, tell the
customer; mention anything else they name in one line at the end.

Report, from what the server returns and nothing from memory:

- **Google Ads**, **Google Tag Manager** and **Google Analytics 4**, the three
  products this plugin works from: connected or not. For one that is not, say
  what connecting it unlocks in the server's own words (its `unlocks`) and give
  its `connect_url`. For one that needs to be signed in again or added again, say
  what its `next_step` says.
- That the Google products are connected inside AdCopilot at
  `https://app.adcopilot.cloud`, not in this AI client. A sign-in that reports
  nothing connected is the server working, not a fault.
- That AdCopilot does not connect to Amazon Ads today, so no Amazon account
  appears here, and nothing is missing.

If the connector does not answer, it is a sign-in. In Claude Code, run `/mcp` and
sign in to `adcopilot`. In Claude on the web, in Cowork or in the desktop app,
open this plugin, then its Connectors tab, and connect `adcopilot` there. There is
one connector and one AdCopilot account whichever AdCopilot plugins are added. In
Claude Code, `/mcp` lists it once, under whichever AdCopilot plugin loaded first,
and the sign-in belongs to that entry. If Claude asks for a sign-in again after an
AdCopilot plugin was added or removed, run `/mcp` and sign in once.

End with the one next step that fits: the `check` command to check Amazon
Attribution tags on Google Ads ads or the Amazon Ad Tag in Tag Manager, or the
`plan` command to plan Amazon campaigns.
