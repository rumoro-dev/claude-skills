---
name: rumoro-triage
description: Triage today's Rumoro mentions from Claude Code. Lists the relevant mentions since yesterday grouped by what needs doing (buying intent, questions, bugs and complaints), drafts replies or GitHub issues for the ones the user picks, and marks mentions done or ignored when asked. Use when the user asks what people are saying about the product, what came in today, or to go through their mentions.
---

# Triage today's mentions

Every tool below comes from the Rumoro MCP server. This skill never posts anything. The user replies by hand.

If the Rumoro tools are missing or answer `Unauthorized`, do not report the sign-in as done: tell the user to run `claude mcp add --transport http rumoro https://mcp.rumoro.dev/mcp`, then `/mcp`, choose `rumoro` and Authenticate, or to add the server with an API key as this repository's README describes.

## 1. See what came in

Call `search_mentions` with `relevant: true`, `status: "open"`, `since` set to the start of yesterday in the user's timezone (ISO 8601), `sort: "priority"` and `limit: 50`. If the user named a platform or a keyword, add `platforms` or `keywordIds`.

Then call `list_keywords` with `kind: ["competitor"]` (no `limit`, so every competitor keyword comes back) and keep their `id`s. A mention whose `keyword.id` is one of them matched a competitor.

Put each mention in one group only, in this order:

1. **Buying intent**: `classification.intents` contains `buy_intent`, or `churn_intent` on a competitor keyword (someone leaving a competitor).
2. **Questions**: `question`.
3. **Bugs and complaints**: `bug_report` or `complaint`, or a review with `review.rating` of 1 or 2.
4. **Everything else**, one line each.

Show one line per mention: the platform (`post.platform`), who wrote it (`author.name` or `author.handle`, with `author.followers` when known), what they said in one sentence, and the link (`post.url`). Add the classifier's `classification.note` in brackets when it helps.

If nothing came back, say so and offer `get_mention_stats` for the last 7 days.

## 2. Act on the mentions the user picks

- **Reply**: draft an answer that helps first, discloses the affiliation when the product comes up, and fits the platform's tone. If the project's `CLAUDE.md` has reply rules and a voice, follow them. Show the draft. Do not post it.
- **Bug report**: if the mention describes a bug in this repository, find the likely place in the code and draft a GitHub issue with a title, steps to reproduce and the link to the mention.
- **Not relevant**: call `update_mention` with `relevant: false` to record the correction.
- **Later**: call `update_mention` with `snoozedUntil` to hide it until a time the user picks.

## 3. Close the loop

When the user says a mention is handled, call `update_mention` with `status: "done"`. When it is noise, use `status: "ignored"`. End with a short summary of how many mentions were handled, ignored and left open.
