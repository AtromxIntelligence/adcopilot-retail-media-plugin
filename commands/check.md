---
description: Check that the Google Ads ads sending shoppers to Amazon carry an Amazon Attribution tag, and check the Amazon Ad Tag in Google Tag Manager.
---

If the customer's question is about Google Ads that send shoppers to Amazon, or
they typed nothing after the command, follow the `retail-media-attribution` skill.
If it is about Amazon's tag on their own website, follow the
`retail-media-tracking` skill.

If the customer typed something after the command, use it to choose the Google
Ads account, the Tag Manager container (a container name, or its GTM id) or the
question to answer: $ARGUMENTS

When the check is done, offer the other one of the two skills if the customer
has not had it, and the `retail-media-traffic` skill when their Amazon ads land on
their own site.
