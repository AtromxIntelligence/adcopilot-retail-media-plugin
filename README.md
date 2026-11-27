# AdCopilot Retail Media

Retail media help in your AI client, through AdCopilot. It works with Amazon Ads:
this plugin gives Claude the steps to read the Amazon Ads profiles you connect and
review them, to check that the Google Ads ads that send shoppers to your Amazon
listings carry an Amazon Attribution tag, to check Amazon's tag on your own website
in Google Tag Manager, to read Amazon-sourced visits in Google Analytics (GA4), and
to plan and write for Amazon Ads. It works through two connectors: the AdCopilot
connector, which reaches your Google products, and an Amazon Ads connector, which
reads your Amazon Ads profiles.

Made by Atromx Intelligence (https://adcopilot.cloud). Not related to other products named AdCopilot. Amazon, Amazon Ads and Amazon Attribution are trademarks of Amazon or its affiliates. We are an independent service provider and are not affiliated with or endorsed by Amazon.

## What it does today

- **Reads your Amazon Ads profiles, read only** (`amazon_get_context`,
  `amazon_profiles`, `amazon_portfolios`, `amazon_campaigns`, `amazon_ad_groups`,
  `amazon_ads`, `amazon_targets`, `amazon_account_overview`, `amazon_search_terms`,
  `amazon_report_result`): campaigns, ad groups, ads and targeting for Sponsored
  Products, Sponsored Brands and display ads, with their status, budgets and bids;
  results such as impressions, clicks, cost, and the sales and orders Amazon
  attributes to your ads; and the search terms shoppers used. One advertising
  profile is one country's marketplace, with its own currency and time zone.
- **Reviews them** (`amazon_full_audit`, `amazon_wasted_spend`,
  `amazon_keyword_harvest`, `amazon_budget_pacing`, `amazon_period_compare`): an
  audit with a score, spend with no attributed sales and the negatives it proposes,
  search terms that sold and could become keywords, this month's pacing, and a
  period against the one before. These are AdCopilot's own checks and arithmetic on
  Amazon's figures. Proposals stay proposals: you make any change in Amazon's
  console.
- **Shows Amazon's own suggestions** (`amazon_bid_advice`, `amazon_budget_advice`):
  Amazon's suggested bids for a Sponsored Products ad group's targets, and its
  suggested daily budgets, with its estimate of what a campaign missed while out of
  budget. Nothing is changed.
- **Checks Amazon Attribution on your Google Ads** (`list_accessible_customers`,
  `get_resource_metadata`, `search`, `performance_report`). It finds the ads and
  Performance Max asset groups whose final URL is an Amazon page, says which carry
  an Amazon Attribution tag and which do not, shows any tracking template or final
  URL suffix on their path, and reports the Google clicks and cost they take.
  Google Ads and Analytics cannot count what a shopper does on Amazon; Amazon
  Attribution can, once the tag is on the ad.
- **Checks your Amazon Ad Tag in Google Tag Manager** (`gtm_list_accounts`,
  `gtm_list_containers`, `gtm_list_workspaces`, `gtm_list_tags`,
  `gtm_list_triggers`, `gtm_list_version_headers`). It finds Amazon's tag, from
  Amazon's gallery template or as code, says which tag id, event and trigger each
  uses and whether it sits in a draft, and names the proof that it fires. When you
  paste the code from your own Amazon Ads console, it proposes a Custom HTML tag
  and, on your yes, stages it (`gtm_create_trigger` and `gtm_create_tag`, with
  `gtm_list_built_in_variables` and `gtm_enable_built_in_variable` for the Page
  Path variable a page filter reads). Staged means in a workspace named AdCopilot,
  never published.
- **Reads Amazon-sourced visits in Google Analytics (GA4)**
  (`ga4_list_account_summaries`, `ga4_get_metadata`, `ga4_run_report`,
  `ga4_list_conversion_events`) by source, medium and campaign, for Amazon ads
  that land on your own site, and finds UTM mistakes.
- **Plans campaigns and drafts Sponsored Brands headlines**: Sponsored Products
  discovery and keyword campaigns by match type, product targets, negatives, a
  weekly search-term routine, Sponsored Brands and display ads (formerly
  Sponsored Display). This is Claude's own work, from your saved business profile
  (`get_org_context`) and, for a restructure, a read-only look at the campaigns you
  run today.
- **Does the ACOS, TACOS and budget math**, with a break-even ACOS from your
  margin, from the figures you give it or, with Amazon Ads connected, from your
  profile's own results. TACOS always needs your total sales, which the Amazon Ads
  reads here do not include.

## Amazon Ads on its own

Every skill and command opens with this, once:

> Today this plugin reads the Amazon Ads profiles you connect (read only: campaigns, ad groups, ads, targeting, budgets, results, search terms and Amazon's own suggested bids and budgets), checks that your Google Ads ads that send shoppers to Amazon carry an Amazon Attribution tag, checks your Amazon Ad Tag in Google Tag Manager, reads Amazon-sourced visits in Google Analytics (GA4), plans campaigns, drafts headlines and does the ACOS and budget math. It changes nothing in your Amazon Ads account.

Amazon Ads figures are reported on their own: never added to, compared with, or put in a table with another platform's figures, and never copied into a spreadsheet or another tool. Amazon makes its figures as reports, so an answer can say that Amazon is still preparing one; Claude then finishes it a little later. Sales, orders, ACOS and ROAS always come with the attribution window they count in, and ACOS and ROAS are calculated by AdCopilot from Amazon's cost and sales. Figures for the last few days can still change.

## The connectors

The plugin is MIT licensed. The AdCopilot connectors it brings are a hosted service, so you need an AdCopilot account to use them.

- `adcopilot`, at https://mcp.adcopilot.cloud/mcp, reaches your Google products:
  Google Ads, Google Tag Manager and Google Analytics.
- `rm`, at https://mcp.adcopilot.cloud/amazon/mcp, reads your Amazon Ads profiles.
  It has its own sign-in. Its first answer says whether your workspace's plan
  includes Amazon Ads.

## Install

**Claude on the web, Cowork and the desktop app.** Open Customize, then Plugins,
search for AdCopilot Retail Media, and Add it. Then open the plugin's Connectors
tab, Connect `adcopilot` and `rm`, and sign in to each.

**Claude Code.** Run `/plugin marketplace add AtromxIntelligence/adcopilot-retail-media-plugin`,
then `/plugin install adcopilot-retail-media@adcopilot-retail-media-plugin`, then
`/mcp` to sign in to `adcopilot` and to `rm`, which Claude Code shows as
`plugin:adcopilot-retail-media:rm`.

**Then, once,** connect your Google products in AdCopilot at https://app.adcopilot.cloud.
For this plugin that is Google Ads, Google Tag Manager and Google Analytics. Which
accounts, containers and properties the plugin can see depends on the Google login
you connect. Then connect your Amazon login: the `setup` command gives you the
link. You choose where you sell (North America; Europe, India, the Middle East and
Africa; or the Far East) and sign in at Amazon on Amazon's own screen. Which
advertising profiles the plugin can read depends on your access to each one at
Amazon, and the workspace owner switches them on in AdCopilot.

## Already use another AdCopilot plugin?

The `adcopilot` connector is one connector and one AdCopilot account, whichever
AdCopilot plugins you add. Every AdCopilot plugin points at the same address for it,
so your AI client treats them as one connector. In Claude Code, `/mcp` shows it once,
under whichever AdCopilot plugin loaded first, and the sign-in belongs to that entry.
If Claude asks you to sign in again after you add or remove an AdCopilot plugin, run
`/mcp` and sign in once. The `rm` connector belongs to this plugin and has its own
sign-in.

## What you get

Four commands. `setup` checks both sign-ins and says what is connected, `review`
follows the `retail-media-review` skill, `check` follows the
`retail-media-attribution` or `retail-media-tracking` skill, and `plan` follows the
`retail-media-plan` skill. In Claude Code they are named
`/adcopilot-retail-media:setup`, `/adcopilot-retail-media:review`,
`/adcopilot-retail-media:check` and `/adcopilot-retail-media:plan`.

Seven skills. `retail-media-connect` connects your Amazon login and passes on the
link, the plan line and any reconnect reason exactly as the Amazon Ads connector
sends them. `retail-media-review` reviews your Amazon Ads profiles.
`retail-media-attribution` checks the Amazon Attribution tags on Google Ads ads that
point to Amazon, `retail-media-tracking` is the Amazon Ad Tag check and staging in
Tag Manager, `retail-media-traffic` is Amazon-sourced visits, key events and UTM
mistakes in Analytics, `retail-media-plan` is the campaign plan and headline drafts,
and `retail-media-budget` is the ACOS and budget math. Every skill that uses a tool
starts with its connector's first call, `get_org_context` or `amazon_get_context`.

Every figure it shows from Google Ads or Analytics is the tool's figure, and every
figure from Amazon Ads is Amazon's, or AdCopilot's arithmetic on Amazon's. A figure
you give it is labelled as yours.

## What it will not do

- Change anything in your Amazon Ads account. The Amazon Ads connector only reads
  here: wasted spend and keyword ideas are proposals for you to make in Amazon's
  console.
- Read Seller Central or Amazon Attribution, or state an Amazon figure it did not
  read or was not given.
- Add Amazon Ads figures to, or compare them with, figures from Google or any other
  platform.
- Edit a Google Ads ad. Putting an Amazon Attribution tag on an ad is your step in
  Google Ads, and the skill names the ads to change.
- Publish Tag Manager. Staged tags wait for your own Submit and Publish.
- Delete anything. Its skills call no tool that deletes or removes.
- Change anything in Google without your explicit yes, after it has told you what
  the change is and how to undo it.
- Check your ads against Amazon's ad policies, or promise a result.

## Data

The plugin is instructions only: it runs no code and sends nothing anywhere itself. The AdCopilot connector at mcp.adcopilot.cloud reads and, on your yes, writes your Google products through Google's APIs with your Google sign-in.

The Amazon Ads connector at mcp.adcopilot.cloud/amazon/mcp reads your Amazon Ads
profiles with your own Amazon sign-in, and changes nothing there. When you ask, it
reads from Amazon a profile's portfolios, campaigns, ad groups, ads and targeting,
with their status, budgets and bids; results such as impressions, clicks, spend, and
the sales and orders Amazon attributes to your ads; the search terms shoppers used,
with anything that looks like an email address or a phone number hidden; and
Amazon's suggested bids and budgets; it does not keep these results.

It keeps, encrypted: your sign-in; each profile's ids, name, type, country, currency, time zone and your access level; the ids of campaigns, ad groups and ads AdCopilot creates, for up to 400 days, so it never switches them on itself; and a 180-day record of which tools ran. It keeps no other campaign data and no results.

Disconnect any time: AdCopilot deletes your sign-in at once. When no one in your workspace is still connected to Amazon, it also deletes your profiles' details and the ids it kept, and its 180-day record of which tools ran no longer names any profile. Amazon asks you to renew this permission once a year. To remove access at Amazon too: remove AdCopilot from the apps allowed to use your Amazon account, in your Amazon account settings.

AdCopilot is AI-powered: what your AI assistant says from these results is
AI-generated and may be wrong, so check it before you act on it. Your AI assistant
is made by a separate company, and its terms and your settings there decide how
long it keeps what it is sent. AdCopilot does not train AI models on Amazon data.

The plugin itself never contacts Amazon. A tag you publish that loads Amazon's
script does so from your own site, in your visitors' browsers, because you chose to
install it. Privacy policy: https://adcopilot.cloud/privacy. Terms:
https://adcopilot.cloud/terms. Documentation: https://adcopilot.cloud/docs/.

## Example prompts

After you sign in, ask in your own words. For example:

- "Audit my Amazon Ads profile: where is the money going, and is the month on pace?"
- "Which Amazon search terms spent money with no sales last month, and which ones sold and should become keywords?"
- "Compare my Sponsored Products results for the last two weeks with the two weeks before."
- "Do my Google Ads that send people to my Amazon listings have Amazon Attribution tags?"
- "Check whether the Amazon Ad Tag is installed in Google Tag Manager, and what fires it."
- "Plan Sponsored Products campaigns for my new product and work out my break-even ACOS."

## Support

Open an issue at https://github.com/AtromxIntelligence/adcopilot-retail-media-plugin/issues,
or write to support@adcopilot.cloud.

To report a security problem, write to support@adcopilot.cloud rather than opening a public issue.

## Version

**0.2.0 (2026-11-25, written against AdCopilot v2.46.44)**: adds the `rm` connector at https://mcp.adcopilot.cloud/amazon/mcp beside `adcopilot`, with twelve reads and five review tools for your Amazon Ads profiles, all read only. New skills `retail-media-connect` and `retail-media-review`, a new `review` command, and `setup` covers both sign-ins. The plan skill can read the campaigns you run today, and the budget skill can read your profile's own spend, sales and pacing. Amazon Ads figures are always reported on their own.

**0.1.0 (2026-11-15, written against AdCopilot v2.46.30)**: the first release. Three commands and five skills that work with Amazon Ads: the Amazon Attribution check on Google Ads ads that point to Amazon, the Amazon Ad Tag check and staging in Google Tag Manager, Amazon-sourced visits and key events in Google Analytics (GA4) with UTM checks, campaign plans and headline drafts, and ACOS and budget math. It reads no Amazon Ads account. Amazon's pages were read on 15 November 2026.

## Licence

MIT. See the LICENSE file.
