# AdCopilot Retail Media

Retail media help in your AI client, through AdCopilot. It works with Amazon Ads:
this plugin gives Claude the steps to check that the Google Ads ads that send
shoppers to your Amazon listings carry an Amazon Attribution tag, to check
Amazon's tag on your own website in Google Tag Manager, to read Amazon-sourced
visits in Google Analytics (GA4), and to plan and write for Amazon Ads. It works
through the AdCopilot connector, which reaches your Google products.

Made by Atromx Intelligence (https://adcopilot.cloud). Not related to other products named AdCopilot. Amazon, Amazon Ads and Amazon Attribution are trademarks of Amazon or its affiliates. We are an independent service provider and are not affiliated with or endorsed by Amazon.

## What it does today

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
  Sponsored Display). This is Claude's own work and uses no tool beyond reading
  your saved business profile (`get_org_context`).
- **Does the ACOS, TACOS and budget math** from the figures you give it, with a
  break-even ACOS from your margin. Claude's own work, with no tool.

## What it does not read

Every skill and command opens with this, once:

> Today this plugin checks that your Google Ads ads that send shoppers to Amazon carry an Amazon Attribution tag, checks your Amazon Ad Tag in Google Tag Manager, reads Amazon-sourced visits in Google Analytics (GA4), plans campaigns, drafts headlines and does the ACOS and budget math. It does not read your Amazon Ads account.

## The connector

The plugin is MIT licensed. The AdCopilot connector it brings is a hosted service, so you need an AdCopilot account to use it.

## Install

**Claude on the web, Cowork and the desktop app.** Open Customize, then Plugins,
search for AdCopilot Retail Media, and Add it. Then open the plugin's Connectors
tab, Connect `adcopilot`, and sign in.

**Claude Code.** Run `/plugin marketplace add AtromxIntelligence/adcopilot-retail-media-plugin`,
then `/plugin install adcopilot-retail-media@adcopilot-retail-media-plugin`, then
`/mcp` to sign in to `adcopilot`.

**Then, once,** connect your Google products in AdCopilot at https://app.adcopilot.cloud.
For this plugin that is Google Ads, Google Tag Manager and Google Analytics. Which
accounts, containers and properties the plugin can see depends on the Google login
you connect.

## Already use another AdCopilot plugin?

There is one connector and one AdCopilot account, whichever AdCopilot plugins you
add. Every AdCopilot plugin points at the same address, so your AI client treats
them as one connector. In Claude Code, `/mcp` shows the connector once, under
whichever AdCopilot plugin loaded first, and the sign-in belongs to that entry. If
Claude asks you to sign in again after you add or remove an AdCopilot plugin, run
`/mcp` and sign in once.

## What you get

Three commands. `setup` checks the connector and says what is connected, `check` follows the `retail-media-attribution` or `retail-media-tracking` skill, and `plan` follows the `retail-media-plan` skill. In Claude Code they are named `/adcopilot-retail-media:setup`, `/adcopilot-retail-media:check` and `/adcopilot-retail-media:plan`.

Five skills. `retail-media-attribution` checks the Amazon Attribution tags on Google Ads ads that point to Amazon, `retail-media-tracking` is the Amazon Ad Tag check and staging in Tag Manager, `retail-media-traffic` is Amazon-sourced visits, key events and UTM mistakes in Analytics, `retail-media-plan` is the campaign plan and headline drafts, and `retail-media-budget` is the ACOS and budget math, with no tool. Every skill that uses a tool starts with `get_org_context`.

Every figure it shows from Google Ads or Analytics is the tool's figure. A figure you give it is labelled as yours.

## What it will not do

- Read your Amazon Ads account or Seller Central, or state an Amazon click, sale,
  spend or ACOS figure it was not given.
- Edit a Google Ads ad. Putting an Amazon Attribution tag on an ad is your step in
  Google Ads, and the skill names the ads to change.
- Publish Tag Manager. Staged tags wait for your own Submit and Publish.
- Delete anything. Its skills call no tool that deletes or removes.
- Change anything without your explicit yes, after it has told you what the change
  is and how to undo it.
- Create or change a campaign on Amazon, check your ads against Amazon's ad
  policies, or promise a result.

## Data

The plugin is instructions only: it runs no code and sends nothing anywhere itself. The AdCopilot connector at mcp.adcopilot.cloud reads and, on your yes, writes your Google products through Google's APIs with your Google sign-in.

The plugin never contacts Amazon. A tag you publish that loads Amazon's script does
so from your own site, in your visitors' browsers, because you chose to install it.
Privacy policy: https://adcopilot.cloud/privacy. Terms:
https://adcopilot.cloud/terms. Documentation: https://adcopilot.cloud/docs/.

## Example prompts

After you sign in, ask in your own words. For example:

- "Do my Google Ads that send people to my Amazon listings have Amazon Attribution tags?"
- "Check whether the Amazon Ad Tag is installed in Google Tag Manager, and what fires it."
- "How many visits and key events did my Amazon DSP ads bring to my site last month?"
- "Plan Sponsored Products campaigns for my new product and work out my break-even ACOS."

## Support

Open an issue at https://github.com/AtromxIntelligence/adcopilot-retail-media-plugin/issues,
or write to support@adcopilot.cloud.

To report a security problem, write to support@adcopilot.cloud rather than opening a public issue.

## Version

**0.1.0 (2026-11-15, written against AdCopilot v2.46.30)**: the first release. Three commands and five skills that work with Amazon Ads: the Amazon Attribution check on Google Ads ads that point to Amazon, the Amazon Ad Tag check and staging in Google Tag Manager, Amazon-sourced visits and key events in Google Analytics (GA4) with UTM checks, campaign plans and headline drafts, and ACOS and budget math. It reads no Amazon Ads account. Amazon's pages were read on 15 November 2026.

## Licence

MIT. See the LICENSE file.
