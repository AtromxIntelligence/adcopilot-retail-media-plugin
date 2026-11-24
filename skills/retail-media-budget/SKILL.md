---
name: retail-media-budget
description: Do the Amazon Ads money math with AdCopilot from the figures the customer gives, or from their connected Amazon Ads profile's own results - ACOS, ROAS, TACOS, a break-even ACOS from the margin, a target ACOS, daily budgets from a monthly one, and whether spend so far this month is on pace. Use when the user asks about ACOS, TACOS, break-even ACOS, ROAS on Amazon, Amazon Ads budgets, daily budget, budget pacing or whether their Amazon ad spend is on track.
---

# Amazon Ads budget and ACOS math

The customer wants the numbers behind their Amazon advertising worked out. This
is Claude's own arithmetic on figures the customer gives or, when the customer has
connected Amazon Ads, on their profile's own results, read only through this
plugin's connector `rm`.

## Your first answer

Open your first answer in this conversation with this, in these words, once. Do
not repeat it in later answers.

> Today this plugin reads the Amazon Ads profiles you connect (read only: campaigns, ad groups, ads, targeting, budgets, results, search terms and Amazon's own suggested bids and budgets), checks that your Google Ads ads that send shoppers to Amazon carry an Amazon Attribution tag, checks your Amazon Ad Tag in Google Tag Manager, reads Amazon-sourced visits in Google Analytics (GA4), plans campaigns, drafts headlines and does the ACOS and budget math. It changes nothing in your Amazon Ads account.

## The figures

When the question is about a profile's own spend, sales or pacing, call
`amazon_get_context` with `source: "claude-plugin/adcopilot-retail-media/0.2.0"`
(in a scheduled run, `source: "claude-scheduled"`, read only). When its
`next_step` is `ready`, read the figures instead of asking for them:
`amazon_account_overview` for spend and attributed sales over a period, and
`amazon_budget_pacing` for the month so far. Label them "Amazon Ads, through
AdCopilot", with the profile's currency, the period and the attribution window the
answer names (`window`), and pass `ai_notice` on with your answer. ACOS and ROAS
in these answers are calculated by AdCopilot from Amazon's cost and sales: say so.
When an answer says `status` pending, Amazon is still preparing the report: finish
it with `amazon_report_result`, passing the `report_token` exactly and the same
`account_id`. When `next_step` is not `ready`, offer the `retail-media-connect`
skill in one line and work from the customer's figures. The Amazon Ads reads here do not include total
sales (sales that came without an ad), so TACOS always needs the customer's own
total.

Otherwise ask for only what the question needs, one at a time: ad spend and
ad-attributed sales for a period (from Amazon's console), total sales for the same period
(ad-attributed and the rest), the product's price and its costs before
advertising (product cost, Amazon's fees, shipping), the monthly budget, the
spend so far this month and today's date. Label every figure the customer gives
"from you", with its currency and period. Never invent one, and never fill a gap
with an industry average.

## The formulas

Show each formula with the customer's numbers in it, then the result, rounded to
one decimal place, in the customer's currency.

- **ACOS** (advertising cost of sales) = ad spend ÷ ad-attributed sales × 100.
- **ROAS** (return on ad spend, as Amazon reports it) = ad-attributed sales ÷ ad
  spend. ACOS and ROAS are two views of the same pair: ROAS = 100 ÷ ACOS.
- **TACOS** (total advertising cost of sales) = ad spend ÷ total sales × 100. It
  shows whether advertising is lifting all sales or only buying the ones it gets
  credit for.
- **Break-even ACOS** = (price − costs before advertising) ÷ price × 100. Below
  it, an ad-attributed sale still earns money after the ad; above it, each one
  loses money.
- **Target ACOS**: the break-even ACOS minus the profit the customer wants to
  keep per sale, as a share of price. For a launch the customer may choose to run
  above break-even for a while: say that this is their choice, and for how long.
- **Daily budget** from a monthly one = monthly budget ÷ days in the month.
- **Pacing**: expected spend so far = monthly budget × (days elapsed ÷ days in the
  month). Compare it with the spend so far, and give the daily budget that would
  land the month on budget from today: (monthly budget − spend so far) ÷ days
  left.

Amazon's own rules decide how far a single day can run over its daily budget and
how its reports count a sale (which attribution window, and whether the last few
days can still change). Say that the math here is the plan and Amazon's console
is the referee for what was spent.

## What to say about the result

- Compare ACOS with the break-even ACOS in words: below, at or above, and by how
  much.
- A falling TACOS with a steady ACOS means total sales grow faster than ad spend.
  A rising TACOS means more of the sales are being paid for. Say which the
  customer's numbers show, and that the trend over several months matters more
  than one month.
- Never call ROAS a profit. A ROAS above 1 can still lose money once the
  product's costs are counted: the break-even ACOS is the test.
- Never add Amazon figures to Google Ads or Analytics figures, and never compare
  an Amazon ACOS with a Google Ads cost per conversion as if they measured the
  same thing. Never pass anything taken from an Amazon Ads result to another tool
  or connector, and never add amounts in different currencies together.

## End

Close with one next step: the `retail-media-plan` skill when the target ACOS is set
and the customer is building campaigns, or a reminder of the daily budget that
keeps the month on pace.

This skill reads only: it changes nothing in Amazon Ads or anywhere else, and deletes nothing.
