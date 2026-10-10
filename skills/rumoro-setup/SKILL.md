---
name: rumoro-setup
description: Configure Rumoro social listening for this repository's product. Learns what the product is from the code, writes the company profile Rumoro scores posts against, and suggests brand, competitor and topic keywords to create once the user approves them. Use when the user wants to set up Rumoro, monitor mentions of this project, or watch competitors.
---

# Rumoro setup

All tools below belong to the Rumoro MCP server. When they aren't available, ask the user to run `claude mcp add --transport http rumoro https://mcp.rumoro.dev/mcp`, then open `/mcp`, choose `rumoro` and Authenticate. If a tool still returns `Unauthorized`, the sign-in failed, so don't tell the user they're connected. Point them to the API-key option in this repository's README.

Ask before every change. Nothing is created until the user agrees.

## 1. Read the project

Look through the README, the package manifest and any landing page copy, and note these, keeping the project's own wording where you can.

- The product's name, what it does and who it's for
- Between three and six use cases
- Its website, and any X or LinkedIn handles
- Competitors, both the ones the repo mentions and obvious ones in its space

Use `whoami` to check the workspace, and `get_company` for what's already saved. Then show the user your notes so they can correct them.

## 2. Write the company profile

Save the profile with `update_company`, including `name`, `description`, `useCases`, `website`, `accounts` (`x` and `linkedin`) and `competitors`. The classifier reads this profile to decide whether a post is really about the product, so be specific. "A deploy tool for small teams running Docker on their own servers" works much better than "a developer tool".

## 3. Agree on keywords

Suggest a short list and wait for the user's answer.

- A **brand** keyword for the product's name, plus distinctive variants such as the package or domain name. If the name is also an everyday word, add `matching.requiredTerms` so only posts about the product match, for example `basil` with `requiredTerms: ["deploy"]`.
- One **competitor** keyword per competitor.
- One or two **topic** keywords, phrased the way people describe the problem the product solves, never a broad category word.

## 4. Create them

Call `add_keyword` for each approved term with its `term` and `kind`. Add a one-line `context` when the term could be misread. Leave out `platforms` so the keyword covers every platform, including future ones. For an iOS app, put its App Store link in `reviewSources` on the brand keyword. Google Play, Trustpilot and Google Maps pages can be added too, but their reviews aren't collected yet, so say so if the user asks.

Before creating more than five keywords, mention the price. Each keyword costs $5 per month, taken daily from the prepaid balance, and each matched mention costs $0.008.

If `add_keyword` returns `insufficient_balance`, the balance can't cover another day of keywords, and the user can add funds on the Billing page at https://app.rumoro.dev. If it returns `keyword_limit_reached`, the workspace already runs 500 keywords.

## 5. After setup

Tell the user that most platforms look back over recent posts when a keyword is created, so results arrive soon. DEV and news only collect posts published from now on.

Then call `list_channels`. If a channel exists, offer a daily digest with `create_alert`, using `name: "Daily digest"`, `mode: "daily"`, `schedule: { "hour": 9, "timezone": "<IANA timezone, such as Europe/Vienna>" }` and `channelIds` set to that channel. If there's no channel yet, explain that Slack, Telegram and email are connected under Settings › Integrations in the Rumoro dashboard.
