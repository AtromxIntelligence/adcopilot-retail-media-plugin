---
name: retail-media-budget
description: Do the Amazon Ads money math with AdCopilot from the figures the customer gives - ACOS, ROAS, TACOS, a break-even ACOS from the margin, a target ACOS, daily budgets from a monthly one, and whether spend so far this month is on pace. Use when the user asks about ACOS, TACOS, break-even ACOS, ROAS on Amazon, Amazon Ads budgets, daily budget, budget pacing or whether their Amazon ad spend is on track.
---

# Amazon Ads budget and ACOS math

The customer wants the numbers behind their Amazon advertising worked out. This
is Claude's own arithmetic on figures the customer gives. AdCopilot has no access
to the customer's Amazon Ads account, so it reads no Amazon spend or sales, and
it calls no tool for this math.

## Your first answer

Open your first answer in this conversation with this, in these words, once. Do
not repeat it in later answers.

> Today this plugin checks that your Google Ads ads that send shoppers to Amazon carry an Amazon Attribution tag, checks your Amazon Ad Tag in Google Tag Manager, reads Amazon-sourced visits in Google Analytics (GA4), plans campaigns, drafts headlines and does the ACOS and budget math. It does not read your Amazon Ads account.

## The figures

Ask for only what the question needs, one at a time: ad spend and ad-attributed
sales for a period (from Amazon's console), total sales for the same period
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
  same thing.

## End

Close with one next step: the `retail-media-plan` skill when the target ACOS is set
and the customer is building campaigns, or a reminder of the daily budget that
keeps the month on pace.

This skill calls no tool and makes no change anywhere. It deletes nothing.
