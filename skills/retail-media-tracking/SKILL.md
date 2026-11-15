---
name: retail-media-tracking
description: Check through AdCopilot whether the Amazon Ad Tag, the site tag behind Amazon DSP conversion tracking and audiences, is set up in Google Tag Manager, name the proof that it fires, and stage the tag when the customer pastes Amazon's code. Use when the user asks about the Amazon Ad Tag, the Amazon Advertising Tag, the Amazon pixel, amzn.js, Amazon DSP conversion tracking, Amazon events on their own website, or whether their Amazon tag fires.
---

# Amazon Ad Tag check in Tag Manager

The customer wants to know whether Amazon's tag is in place on their own website,
the tag Amazon uses to count conversions and build audiences from visits to sites
outside Amazon. This skill reads their Google Tag Manager through the AdCopilot
connector at `https://mcp.adcopilot.cloud/mcp` and, on their yes, stages tags
there. AdCopilot has no access to the customer's Amazon Ads account and reads no
Amazon figure; the tag is checked from the tags in Tag Manager, and what only
Amazon's own screens can show is named below.

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

If the card for Google Tag Manager under `products` is not connected, say what
connecting it unlocks, in the server's own words (its `unlocks`), give its
`connect_url`, and stop. If the card says it needs to be signed in again or added
again, say what its `next_step` says and stop. Never give a link from memory, and
never describe a container you cannot read.

## What the Amazon Ad Tag is

From Amazon's own Tag Manager template page, read 15 November 2026:

- Amazon publishes a template called "Amazon Advertising Tag" in Tag Manager's
  Community Template Gallery. A tag built from it takes one or more tag ids that
  the customer copies from their Amazon Ads console, an event name (a standard
  one such as PageView, AddToShoppingCart or Checkout, or a custom name), and the
  region the tag reports to. It also has consent and matching options.
- The tag's script is Amazon's `amzn.js`.

And at the time of writing:

- The same tag can be added as code: the script Amazon gives loads `amzn.js`
  from Amazon's ad server (its address contains `amazon-adsystem`) and calls a
  function named `amzn`. The code in the customer's own console is the referee.
- Amazon also offers a server-to-server route for conversions, which needs no tag
  on the page. If the customer uses only that, nothing here will show it.

## Read the container

1. `gtm_list_accounts`, then `gtm_list_containers` for the account. Pick the
   container that is on the customer's site. If there is more than one, ask, by
   display name. You cannot see their website, so say that the container is the
   one they named, not one you confirmed on the page.
2. `gtm_list_workspaces` for the container. Read the Default Workspace, any
   workspace the customer names, and the workspace named AdCopilot if there is
   one: it holds what AdCopilot staged earlier.
3. `gtm_list_tags` and `gtm_list_triggers` for each workspace you read.
4. `gtm_list_version_headers` for the container.

Find Amazon's tag among the tags:

- a tag built from a gallery template (its type starts with `cvt_`) whose name or
  parameters show Amazon's Advertising Tag: read its tag ids and event name as the
  server returned them;
- a Custom HTML tag (type `html`) whose code loads `amzn.js` from an address that
  contains `amazon-adsystem`, or calls `amzn(`.

When you cannot tell what a tag sends, say so and send the customer to Amazon's
screens below.

Report each finding with its evidence (tag name, workspace, trigger name, as the
server returned them), and never a number or a name you did not read:

- **The page-view tag.** Which tag sends a page view to Amazon, with which tag
  id, and on which trigger. Tag Manager's built-in All Pages trigger (id
  2147479553) fires it on every page. If no tag in the workspaces you read sends
  one, say so. The code can also sit directly in the site's own pages, which Tag
  Manager cannot show, so do not say the tag is missing.
- **Event tags.** For each tag that sends another event (an add-to-cart, a
  checkout or a custom name): the event, the tag id and the trigger. An event tag
  whose tag id differs from the page-view tag's is worth a question: Amazon
  counts it under that other id.
- **Two copies.** Two tags that send the same event with the same tag id on the
  same pages count it twice. Ask which is meant.
- **No trigger or paused.** A tag with no firing trigger does not fire on its
  own, and a tag marked paused does not fire either, if the tag shows it.
- **Draft or published.** The tags a workspace lists can include changes nobody
  has published. AdCopilot can list the container's versions (their names and
  ids, as the server returns them) but cannot open one to see its tags, and it has
  no tool that says which version is live. So it can say what a workspace holds
  and when the latest version was made, and it cannot prove a tag is live.
- **Consent.** If a tag shows a consent setting, say what it shows. AdCopilot
  cannot see the site's cookie banner or any visitor's choice.

## The proof that it fires

What you read is a list of tags. You did not watch a page load, so name the
proof:

- Amazon's own tag screens in the Amazon Ads console show whether events arrive
  for each tag id. Amazon renames and moves its screens, so the customer opens
  the console and looks for its ad tag or events section there.
- Tag Manager's own Preview mode shows which tags fired on a page (at the time of
  writing).

AdCopilot cannot see either. Say so, and say which one to open first.

## When the customer pastes Amazon's code

Staging a tag is a change, so it follows the rule at the end of this skill.

- Use only what the customer pastes from their own Amazon Ads console. Never
  type, complete or correct Amazon's code from memory or from this skill, and
  never supply a tag id you were not given.
- Read the paste before you propose anything. It should load `amzn.js` from an
  address that contains `amazon-adsystem` and call `amzn`. If it holds anything
  else, such as another script address or other inline code, say what and do not
  stage it: send the customer back to their Amazon Ads console for the code.
- If the customer would rather use Amazon's gallery template, say that adding a
  template to a container is their click in Tag Manager (Templates, then Search
  Gallery, at the time of writing), and that AdCopilot stages only Custom HTML
  tags from pasted code.
- Propose, in plain words: one Custom HTML tag named "Amazon Ad Tag" with the
  pasted code, firing on All Pages for a page-view tag; for an event on one page,
  a Page View trigger (type `pageview`) filtered to the page path the customer
  names, with the tag on that trigger. Say that both are staged in a workspace
  named AdCopilot, that nothing fires until the customer reviews and publishes it
  in Tag Manager (Submit, then Publish, at the time of writing), and that to undo
  it they leave it unpublished or delete it in that workspace.
- A filter on the page path reads Tag Manager's built-in Page Path variable,
  which must be enabled in the workspace for the filter to work. Read
  `gtm_list_built_in_variables` first, for the workspace named AdCopilot if there
  is one, or else the Default Workspace. If Page Path is not enabled, include
  `gtm_enable_built_in_variable` for it in the same proposal.
- On their yes, make the trigger first with `gtm_create_trigger` when one is
  needed, then the tag with `gtm_create_tag`: type `html`, the pasted code in the
  parameter `html`, and the trigger's id as its firing trigger. If the container
  already has a Custom HTML tag, `gtm_list_tags` shows the parameter keys it uses;
  copy them.
- Read the result back with `gtm_list_tags` and `gtm_list_triggers` for the
  AdCopilot workspace (`gtm_list_workspaces` finds it) and report what you read.
  Do not say it works: the proof is Preview and Amazon's own tag screens.
- Tell the customer the tag fires wherever its trigger does. If the site asks
  visitors for consent before advertising tags, their consent setup decides, and
  AdCopilot does not read it.
- Never publish, and never delete: the connector has no Tag Manager tool for
  either.

## End

Close with one next step: when the tag is in place, offer the `retail-media-traffic`
skill to see Amazon-sourced visits in Analytics, or the `retail-media-attribution`
skill when the customer's Google Ads send shoppers to Amazon.

Every change is proposed first, with what it changes and how to undo it, and is applied only after the customer's explicit yes in this conversation. A change to Tag Manager is staged in the AdCopilot workspace and never published. This skill deletes nothing.
