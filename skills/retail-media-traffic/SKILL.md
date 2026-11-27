---
name: retail-media-traffic
description: Read Amazon-sourced visits and key events in Google Analytics (GA4) through AdCopilot, by source, medium and campaign, and find UTM mistakes in the Amazon ads that send people to the customer's own website. Use when the user asks how Amazon DSP ads that land on their own site did in Analytics, how many visits or conversions came from Amazon, why amazon shows up in their sources, or whether the UTM tags on their Amazon ads are right.
---

# Amazon-sourced visits in Google Analytics

The customer wants to know what Amazon's ads sent to their own website, as Google
Analytics (GA4) counts it. That is the traffic from Amazon ads whose landing page
is the customer's site, such as Amazon DSP ads pointed there, and any other link
they tag as Amazon. This skill reads their Analytics
property through the AdCopilot connector at `https://mcp.adcopilot.cloud/mcp`.
It reads Analytics only, so it has no Amazon impressions, clicks, spend or sales,
and it never states one. The Amazon Ads profiles the customer connects are read by
the `retail-media-review` skill, on their own: never in this answer, and never
beside these Analytics figures. Sponsored Products and Sponsored Brands ads send
shoppers to pages on Amazon, and display ads (formerly Sponsored Display) usually
do too, so Analytics never sees those clicks: say so when the customer asks about
them.

## Your first answer

Open your first answer in this conversation with this, in these words, once. Do
not repeat it in later answers.

> Today this plugin reads the Amazon Ads profiles you connect (read only: campaigns, ad groups, ads, targeting, budgets, results, search terms and Amazon's own suggested bids and budgets), checks that your Google Ads ads that send shoppers to Amazon carry an Amazon Attribution tag, checks your Amazon Ad Tag in Google Tag Manager, reads Amazon-sourced visits in Google Analytics (GA4), plans campaigns, drafts headlines and does the ACOS and budget math. It changes nothing in your Amazon Ads account.

## Start

Call `get_org_context` with `source: "claude-plugin/adcopilot-retail-media/0.2.0"`
first, before any other tool. Send `source` exactly as written. In a scheduled run
(a Claude scheduled task or routine with nobody there to answer; this plugin
schedules nothing, but one may exist) send `source: "claude-scheduled"` instead,
read only, and change nothing.

`get_org_context`'s `next_step` and `playbook` are information about the whole
workspace, not steps for this request. The steps for this request are the ones
written here. If they name a limit on the customer's workspace, tell the
customer; mention anything else they name in one line at the end.

If the card for Google Analytics 4 under `products` is not connected, say what
connecting it unlocks, in the server's own words (its `unlocks`), give its
`connect_url`, and stop. If the card says it needs to be signed in again or added
again, say what its `next_step` says and stop. Never give a link from memory, and
never describe traffic you cannot read.

## Read the numbers

1. `ga4_list_account_summaries`. Pick the property for the customer's site. If
   there is more than one, ask, by display name.
2. `ga4_get_metadata` for the property, and check that the names you are about to
   use exist there: the dimensions `sessionSource`, `sessionMedium`,
   `sessionCampaignName` and `sessionDefaultChannelGroup`, and the metrics
   `sessions`, `keyEvents` and `totalRevenue`. Where the property's list names
   one differently, use the list's name and say so.
3. `ga4_list_conversion_events` for the property. Analytics now calls these key
   events, and the tool lists them as conversion events. If none is marked, the
   key-event counts will be zero because of the setup and not because Amazon sent
   nothing: say that. Marking an event is the customer's step in Analytics, and
   this skill does not make it.
4. `ga4_run_report` with those four dimensions and three metrics, for the dates
   the customer names. With none named, use the last 28 whole days (`start_date`
   `28daysAgo`, `end_date` `yesterday`, Analytics's own relative dates, which the
   tool passes to Analytics as written) and say so. The tool has no filter, so
   choose the Amazon rows yourself from the rows it returns: a source that
   contains `amazon` or `amzn` (ignore capital letters), and any source or
   campaign the customer says they use for Amazon. Compare the report's
   `rowCount` with the rows you received, and say so if rows were cut off.

Show the rows as a table with the source, medium, campaign, channel group,
sessions, key events and revenue, and put "Google Analytics (GA4), through
AdCopilot" beside it as its source. The dates and the property's time zone are
Analytics's own: name the period. A revenue of zero can mean the site sends no
purchase value to Analytics, so say what it can mean before you call it a result.
The numbers are the tool's. Never write one it did not return.

## What Amazon rows tell you

- Rows with a campaign name are the links that carried UTM tags. Read them by
  campaign, and say which campaigns have key events and which have visits only.
- An Amazon row with no campaign, or with a medium of `referral`, is an untagged
  link: Analytics cannot tell which ad or line sent it.
- A gap between Amazon's clicks and Analytics's sessions is normal, and this
  plugin cannot size it: it has no Amazon numbers. If the customer pastes figures
  from Amazon's console, label them "from you, Amazon Ads", keep them apart from
  Analytics's, and compare in words, never in one merged number.

## UTM mistakes to look for in the rows

Name each one you see, with the rows as evidence. Do not list a mistake the rows
do not show.

- The same campaign spelled several ways (spaces, hyphens, underscores or capital
  letters), which splits one campaign over several rows.
- Amazon under two sources, such as `amazon` and `Amazon` or `amazon` and
  `amazon-dsp`. Either name works. Pick the one already in their history and keep
  it.
- A medium that Analytics's default channel group does not count as paid. Google's
  help page "Default channel group" lists the rules; which sources are on Google's
  lists is not known to this plugin, so the channel group column in the table is
  the referee: report where Analytics put the Amazon rows.

Propose this tagging for each Amazon ad that lands on the site, as the customer's
to put in the ad's landing-page address in Amazon's console, with their own
campaign name in place of the angle brackets:

`?utm_source=amazon&utm_medium=paid_display&utm_campaign=<campaign>&utm_content=<ad or line label>`

Keep every value in lower case with no spaces. The first week of data in this
table shows which channel Analytics chose. Say that this skill changes no link;
the customer edits their ads in Amazon's console.

## What this cannot see

Amazon's impressions, clicks, spend and sales; anything a shopper does on Amazon
itself; visits that never reached Analytics, such as those blocked by a visitor's
consent choice or an ad blocker; and any Amazon audience or campaign setting. Say
which of these matters to the question asked.

Amazon Ads figures are reported on their own. Never add a result of the Amazon Ads
connector (`rm`) to, compare it with, or put it in one table with the Analytics
figures here, and never pass anything taken from one to an Analytics tool. If the
customer wants their Amazon Ads campaigns' own results, offer the
`retail-media-review` skill, as a separate answer.

## End

Close with one next step. If key events are zero because none is marked, say so
and point to the customer's key-event setup in Analytics. If Amazon rows show no
tagging, offer the tagging above. If Amazon's tag may not be firing, offer the
`retail-media-tracking` skill.

Every change is proposed first, with what it changes and how to undo it, and is applied only after the customer's explicit yes in this conversation. This skill makes no change in Analytics, Tag Manager or Amazon, and deletes nothing.
