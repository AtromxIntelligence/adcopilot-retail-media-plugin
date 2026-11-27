---
description: Review your connected Amazon Ads profiles, read only, with an audit, wasted spend, budget pacing, keyword ideas from search terms and Amazon's own suggested bids and budgets.
---

Follow the `retail-media-review` skill.

If the customer typed something after the command, use it to choose the profile,
the campaign, the dates or the question, and do not ask for it again: $ARGUMENTS

Offer the `retail-media-budget` skill when the customer wants the target ACOS or the
budget worked out from what the review showed.

If `amazon_get_context` is not among your tools, that proves nothing, because the connector's tool list can be cached or out of date: call it anyway and believe what it says. If it cannot be called, the sign-in to `rm` is not done: say so, give the sign-in steps for the client in use (the `setup` command lists them), and never describe an Amazon Ads profile you did not read.
