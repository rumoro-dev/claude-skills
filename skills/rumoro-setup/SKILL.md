---
name: rumoro-setup
description: Connect this codebase's product to Rumoro social listening. Works out what the product is from the repository, writes the company profile Rumoro scores posts against, and suggests brand, competitor and topic keywords to create after the user approves them. Use when the user wants to set up Rumoro, start monitoring mentions of this project, or keep an eye on competitors.
---

# Rumoro setup

The tools named here belong to the Rumoro MCP server. If they're unavailable, ask the user to run `claude mcp add --transport http rumoro https://mcp.rumoro.dev/mcp`, then open `/mcp`, pick `rumoro` and Authenticate. If a tool still returns `Unauthorized`, the sign-in didn't happen: point the user to the API-key option in this repository's README instead.

Ask before every change. Nothing is created until the user says yes.

## 1. Read the project

Look through the README, the package manifest and any landing-page copy, and note:

- the product's name, and in a sentence or two, what it does and for whom
- a handful of use cases (three to six)
- the website, plus X or LinkedIn handles if the repo mentions them
- competitors, whether named or obvious from the space

Then call `whoami` (which workspace this is) and `get_company` (what's already saved), and show the user your notes so they can fix anything wrong.

## 2. Write the company profile

Save it with `update_company`: `name`, `description`, `useCases`, `website`, `accounts` (`x`, `linkedin`) and `competitors`. Rumoro scores every post against this profile, so precise beats broad. "A deploy tool for small teams running Docker on their own servers" gets better results than "a developer tool".

## 3. Agree on keywords

Suggest a short list and wait for the user's answer:

- **brand**: the product's name and any distinctive variants, such as the package or domain name. A name that's also an everyday word needs `matching.requiredTerms`, for example `driftwood` with `requiredTerms: ["deploy"]`.
- **competitor**: one keyword per competitor.
- **topic**: one or two phrases people type when they have the problem, never a whole category.

## 4. Create them

Call `add_keyword` for each approved term with its `term` and `kind`, and add a one-line `context` when the term could be misread. Leave out `platforms` so the keyword covers every platform, including future ones. For an iOS app, put its App Store link in the brand keyword's `reviewSources`. Google Play, Trustpilot and Google Maps pages can be added too, but their reviews aren't collected yet; say so if asked.

Pricing to mention before creating more than five: $5 per keyword per month, taken daily from the prepaid balance, plus $0.008 for each matched mention.

Two errors to expect: `insufficient_balance` means the balance won't cover another day of keywords (the user can add funds on the Billing page at https://app.rumoro.dev), and `keyword_limit_reached` means 500 keywords are already running.

## 5. After setup

Tell the user that most platforms look back over recent posts when a keyword is created, so results start arriving soon; DEV and news only collect posts published from now on.

Finally, call `list_channels`. If there's a channel, offer a morning summary: `create_alert` with `name: "Daily digest"`, `mode: "daily"`, `schedule: { "hour": 9, "timezone": "<the user's IANA timezone>" }` and `channelIds` set to it. With no channel yet, explain that Slack, Telegram and email are connected under Settings › Integrations in the Rumoro dashboard.
