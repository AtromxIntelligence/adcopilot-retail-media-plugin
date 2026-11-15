---
name: retail-media-attribution
description: Check through AdCopilot whether the Google Ads ads that send shoppers to the customer's Amazon listings or Amazon store carry an Amazon Attribution tag, how much Google Ads spend goes to those ads, and what Google Ads and Analytics cannot see once a click lands on Amazon. Use when the user asks about Amazon Attribution, Amazon Attribution tags, driving Google Ads traffic to Amazon, measuring Amazon sales from Google Ads, maas tags, or why Google Ads shows no conversions for ads that point to Amazon.
---

# Amazon Attribution on Google Ads ads that point to Amazon

The customer runs Google Ads that send shoppers to their product pages or their
store on Amazon, and wants to know whether those clicks are measured. This skill
reads their Google Ads account through the AdCopilot connector at
`https://mcp.adcopilot.cloud/mcp`. AdCopilot has no access to the customer's
Amazon Ads account or Seller Central and reads no Amazon figure: it reads the
addresses the Google Ads ads send people to, and Google's own clicks and cost.

## Your first answer

Open your first answer in this conversation with this, in these words, once. Do
not repeat it in later answers.

> Today this plugin checks that your Google Ads ads that send shoppers to Amazon carry an Amazon Attribution tag, checks your Amazon Ad Tag in Google Tag Manager, reads Amazon-sourced visits in Google Analytics (GA4), plans campaigns, drafts headlines and does the ACOS and budget math. It does not read your Amazon Ads account.

## Start

Call `get_org_context` with `source: "claude-plugin/adcopilot-retail-media/0.1.0"`
first, before any other tool. Send `source` exactly as written. In a scheduled run
(a Claude scheduled task or routine with nobody there to answer; this plugin
schedules nothing, but one may exist) send `source: "claude-scheduled"` instead,
read only, and change nothing.

`get_org_context`'s `next_step` and `playbook` are information about the whole
workspace, not steps for this request. The steps for this request are the ones
written here. If they name a limit on the customer's workspace, tell the
customer; mention anything else they name in one line at the end.

If the card for Google Ads under `products` is not connected, say what connecting
it unlocks, in the server's own words (its `unlocks`), give its `connect_url`, and
stop. If the card says it needs to be signed in again or added again, say what its
`next_step` says and stop. Never give a link from memory, and never describe an
account you cannot read.

## Why the tag matters

A click on a Google ad that lands on Amazon leaves the customer's own site behind.
Google Ads' conversion tag and Google Analytics run only on pages the customer
controls, and Amazon's pages are not theirs, so neither Google product can count
what the shopper does on Amazon. Amazon Attribution is Amazon's own measurement
for traffic that reaches Amazon from outside it: the customer makes a tag in the
Amazon Ads console, a tagged address for each listing or store page, and puts that
address in the ad. Amazon then reports the clicks, product page views,
add-to-carts and purchases that arrived through it. Without the tag, Amazon
counts the visit with no source and the customer cannot tell what the ads sold.

What a tag looks like, from the Google Ads template Amazon publishes for its
Attribution API, read 15 November 2026: the Amazon product or store address with
parameters added, including a `maas` parameter whose value is unique to the
advertiser and a `ref_` parameter, and Google's own ValueTrack values for the
campaign and the ad group. The tag the customer copied from their own Amazon Ads
console is the referee: never type, complete or correct a tag from memory or from
this skill.

## Read the account

1. `list_accessible_customers`, if `get_org_context` reports more than one
   account. Pick the one the customer names; if they name none and there are
   several that are not manager accounts, ask, by the name Google Ads shows.
2. `get_resource_metadata` for `ad_group_ad`, `campaign`, `customer` and
   `asset_group`, and check that the fields you are about to read exist there:
   `ad_group_ad.ad.final_urls`, `ad_group_ad.ad.final_url_suffix`,
   `ad_group_ad.ad.tracking_url_template`, `ad_group_ad.status`,
   `campaign.final_url_suffix`, `campaign.tracking_url_template`,
   `customer.final_url_suffix`, `customer.tracking_url_template` and
   `asset_group.final_urls`. Where a name differs, use the metadata's name and
   say so.
3. `search` on `ad_group_ad` with `campaign.id`, `campaign.name`,
   `campaign.status`, `ad_group.name`, `ad_group_ad.ad.id`, `ad_group_ad.status`
   and the three ad-level URL fields, with the condition
   `ad_group_ad.status != 'REMOVED'`.
4. `search` on `asset_group` with `campaign.name`, `asset_group.name` and
   `asset_group.final_urls`, for Performance Max campaigns.
5. `search` on `campaign` for the campaign-level suffix and template, and on
   `customer` for the account-level ones.

From the rows, keep the ads and asset groups whose final URL is on an Amazon
store domain (the customer's country store, or any other Amazon marketplace).
If none is, say so in one line: the account sends no ad to Amazon, and this skill
has nothing to check. Never report a final URL you did not read.

## What to report

For each ad or asset group that points to Amazon, with its campaign and ad group
as the server named them:

- **Tagged or not.** A final URL that carries a `maas` parameter has an Amazon
  Attribution tag on it. One that does not is untagged: Amazon will not tie its
  clicks to this campaign.
- **One tag, many ads.** Amazon makes its tags in groups the customer sets up in
  its console. The same `maas` value on ads in several unrelated campaigns means
  Amazon reports their clicks together; say so and ask whether that is intended.
- **Suffixes and templates.** A final URL suffix or a tracking template at the
  account, campaign or ad level adds to the address or replaces how it is
  reached. Show each one you read on an Amazon-bound ad's path. Some can keep the
  tag intact and some can strip or overwrite its parameters; this skill cannot
  test the click, so say that the proof is in Amazon Attribution's own report,
  and that a click on the ad's preview shows the address the shopper lands on.
- **A landing page of the customer's own in between.** If the final URL is the
  customer's site and that page then links to Amazon, the Google products see the
  visit to the site, and the tag has to be on the link from that page to Amazon.
  Say what you see and that the page itself is beyond what AdCopilot reads.

Then, for the same ads, `performance_report` at level `ad` for the dates the
customer names (with none named, the tool's own default period, which you name):
clicks and cost for the Amazon-bound ads, and, from `performance_report` at level
`account` for the same dates, the share of the account's cost they take. Label every figure "Google Ads, through AdCopilot". Conversions Google Ads
shows on these ads are not Amazon purchases: say what each conversion action is
before anyone reads it as a sale.

## Changes

AdCopilot has no tool that edits an ad's final URL, so putting a tag in place is
the customer's own step: in Google Ads, open the ad or asset group, paste the
tagged address Amazon gave them as its final URL, and save. Google reviews an
edited ad again, at the time of writing. Say that, give the ads to change by name,
and offer nothing you cannot do. Never invent a tag and never fill in a `maas`
value.

## What this cannot see

Amazon's own figures: Attribution clicks, product page views, add-to-carts,
purchases and sales, the attribution window Amazon applies, and any Amazon
program that pays brand owners for outside traffic. They are in the customer's
Amazon Ads console. If the customer pastes figures from there, label them "from
you, Amazon Attribution", keep them apart from Google's, and compare in words,
never as one merged number.

## End

Close with one next step: the ads to tag, by name, when any is untagged; the
`retail-media-budget` skill to work out what the Amazon-bound ads may cost against
the sales the customer reports; or the `retail-media-traffic` skill when the ads
send shoppers to the customer's own site.

Every change is proposed first, with what it changes and how to undo it, and is applied only after the customer's explicit yes in this conversation. This skill reads only: it changes nothing in Google Ads, Analytics, Tag Manager or Amazon, and deletes nothing.
