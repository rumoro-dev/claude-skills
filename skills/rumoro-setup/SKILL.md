---
name: rumoro-setup
description: Set up Rumoro social listening for the product in this repository. Reads the repo to learn what the product is, fills in the Rumoro company profile, proposes brand, competitor and topic keywords, and creates them once the user agrees. Use when the user asks to set up Rumoro, track mentions of this project, or monitor competitors.
---

# Set up Rumoro for this product

Every tool below comes from the Rumoro MCP server. If it is not connected, ask the user to run `claude mcp add --transport http rumoro https://mcp.rumoro.dev/mcp`, then `/mcp`, choose `rumoro` and Authenticate in the browser. If a tool still answers `Unauthorized`, do not report the sign-in as done: tell the user they can add the server with an API key instead, as this repository's README describes.

Nothing is created or changed without the user's yes.

## 1. Learn the product

Read the repository's README, package manifest and any landing page copy. Write down, in the user's own words where you can:

- the product name and two sentences on what it does and who it is for
- three to six use cases
- the website, and the product's X and LinkedIn handles if they appear
- competitors the repo names or clearly competes with

Call `whoami` to confirm the workspace and `get_company` to see what is already saved. Show the user your draft and let them correct it.

## 2. Fill in the company profile

Call `update_company` with `name`, `description`, `useCases`, `website`, `accounts` (`x`, `linkedin`) and `competitors`. Rumoro's classifier reads this profile to decide whether a post is about the company, so be specific. "An open-source Postgres migration CLI for TypeScript teams" works far better than "a developer tool".

## 3. Propose keywords, then create them

Propose a list and wait for a yes:

- **brand**: the product name, plus distinctive variants such as the package name or domain. If the name is also a common word, add `matching.requiredTerms` so only relevant posts match (for example `arc` with `requiredTerms: ["browser"]`).
- **competitor**: one keyword per competitor.
- **topic**: at most two phrases people use when they have the problem the product solves, not broad category words.

For each agreed keyword, call `add_keyword` with `term` and `kind`, plus a one-sentence `context` when the term is ambiguous. Leave `platforms` out so the keyword tracks every platform, including ones Rumoro adds later. If the product has an iOS app, add its App Store link to the brand keyword's `reviewSources`. Google Play, Trustpilot and Google Maps pages can be connected too, but their reviews are not collected yet, so say so if the user asks.

Each keyword costs $5 per month, charged daily from the prepaid balance, and each matched mention costs $0.008. Say this before creating more than five keywords.

If `add_keyword` returns `insufficient_balance`, the balance does not cover a day of keywords; tell the user to add funds on the Billing page at https://app.rumoro.dev. `keyword_limit_reached` means the workspace has hit the 500 running keyword limit.

## 4. Tell the user what happens next

A new keyword reads back recent posts on most platforms, so the first matches arrive soon after it is created. DEV and news collect only from the moment the keyword starts.

## 5. Offer one alert

Call `list_channels`. If a channel exists, offer a daily digest: `create_alert` with `name: "Daily digest"`, `mode: "daily"`, `schedule: { "hour": 9, "timezone": "<the user's IANA timezone>" }` and `channelIds` set to that channel. If there is no channel, tell the user they can connect Slack, Telegram or email under Settings › Integrations in the Rumoro dashboard.
