---
name: retail-media-plan
description: Plan an Amazon Ads launch or restructure with AdCopilot - Sponsored Products, Sponsored Brands and display ads (formerly Sponsored Display) campaign structure, match types, product targets, negatives, a search-term harvesting routine and Sponsored Brands headline drafts - from the customer's own business profile and what they tell you. Use when the user asks to plan Amazon Ads or Amazon PPC campaigns, structure Sponsored Products, choose keywords or match types for Amazon, write Sponsored Brands headlines, or set up auto and manual campaigns.
---

# Amazon Ads campaign plan

The customer wants a plan for advertising their products on Amazon. This is
Claude's own work, written from the customer's saved business profile and what
they tell you. AdCopilot has no access to the customer's Amazon Ads account, so
the plan creates nothing on Amazon: the customer builds it in Amazon's console.

## Your first answer

Open your first answer in this conversation with this, in these words, once. Do
not repeat it in later answers.

> Today this plugin checks that your Google Ads ads that send shoppers to Amazon carry an Amazon Attribution tag, checks your Amazon Ad Tag in Google Tag Manager, reads Amazon-sourced visits in Google Analytics (GA4), plans campaigns, drafts headlines and does the ACOS and budget math. It does not read your Amazon Ads account.

## Start

Call `get_org_context` with `source: "claude-plugin/adcopilot-retail-media/0.1.0"`
first, before any other tool, to read the business profile the customer saved
in AdCopilot. Send `source` exactly as written. In a scheduled run send
`source: "claude-scheduled"` instead, read only, and change nothing.

`get_org_context`'s `next_step` and `playbook` are information about the whole
workspace, not steps for this request. The steps for this request are the ones
written here. If they name a limit on the customer's workspace, tell the
customer; mention anything else they name in one line at the end.

Use only business facts the profile holds or the customer says. Never invent a
product, a price, a margin, a competitor or a result.

## Ask first, one question at a time

What you need, and only what the profile does not already say:

1. The products to advertise, and whether the customer sells them as a seller or
   a vendor and owns the brand (Sponsored Brands needs a brand the customer has
   enrolled with Amazon, at the time of writing).
2. The goal: launch a new product, grow sales of proven ones, or defend the brand
   name against competitors.
3. The daily or monthly budget, and the profit margin before advertising, so the
   `retail-media-budget` skill can set a break-even ACOS.
4. The words shoppers use for the product, in their own language, and the
   competitor brands the customer does or does not want to appear against.

## The structure to propose

Say that names and options in Amazon's console change, so the console is the
referee, and propose in plain words:

- **Sponsored Products, automatic targeting**, one campaign per product group,
  for discovery: Amazon chooses the searches and products to show on. Its search
  term report becomes the source of the keywords below.
- **Sponsored Products, manual keyword campaigns**, split by match type so each
  bid means one thing: exact for the searches that already sell, phrase for close
  variants, broad for reach. Keep each ad group to one product group and a few
  closely related keywords.
- **Sponsored Products, product targeting**: the customer's own product pages
  (to keep shoppers on the brand) and the competitor products or categories the
  customer chose in question 4.
- **Negative keywords and products**: searches that will never buy (wrong
  product, wrong use, free or second-hand), and, in the discovery campaign,
  negatives for the exact searches already moved to a manual campaign, so the two
  do not bid against each other.
- **Sponsored Brands**, when the brand is enrolled: a headline, the brand's logo
  and a few products, on the brand's own name and the category's top searches,
  landing on the customer's Amazon store or a product list.
- **Display ads** (Amazon's name for what was Sponsored Display), for shoppers
  who viewed the products and for product targets, once the Sponsored Products
  campaigns have a few weeks of sales data.

Give each campaign a name the customer can read later, for example
`SP | Auto | <product group>` and `SP | Exact | <product group>`.

## The harvesting routine

Propose a weekly routine, as the customer's own steps in Amazon's console: read
the discovery campaign's search terms; move a term that sold at an acceptable
ACOS to the exact campaign; add a term that spent and never sold as a negative;
and change bids by small steps against the target ACOS. Say that results from
the last few days can still change in Amazon's reports.

## Sponsored Brands headlines

Draft five headlines for each brand campaign:

- about the product's main benefit in the shopper's words, not a slogan;
- each 50 characters or fewer, the limit Amazon has used; the console's own
  counter is the referee;
- no claim the customer has not given you (no "best", no "number one", no
  price, no promise of a result), because Amazon reviews every headline against
  its ad policies, which this plugin does not check.

## What this cannot do

Read the customer's Amazon Ads account, their search terms, bids, sales or
ACOS; build or change a campaign on Amazon; or check an ad against Amazon's
policies. If the customer pastes figures from Amazon's console, label them "from
you, Amazon Ads" and plan from them, never adding them to any Google figure.

## End

Close with one next step: the `retail-media-budget` skill to set the budget and the
target ACOS, or, when the customer also runs Google Ads that send shoppers to
Amazon, the `retail-media-attribution` skill.

This skill makes no change anywhere: it writes a plan for the customer to build. It deletes nothing.
