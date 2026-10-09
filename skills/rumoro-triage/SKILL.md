---
name: rumoro-triage
description: Work through new Rumoro mentions inside Claude Code. Pulls the relevant posts from the last day, sorts them into buying signals, questions and problems, drafts replies or GitHub issues for the ones the user chooses, and records what was handled. Use when the user asks what's new, what people are saying about the product, or wants to clear their mentions.
---

# Rumoro triage

The tools named here belong to the Rumoro MCP server. Nothing is ever posted for the user; they publish replies themselves.

If the Rumoro tools are missing or return `Unauthorized`, the sign-in didn't happen: ask the user to run `claude mcp add --transport http rumoro https://mcp.rumoro.dev/mcp`, then `/mcp`, pick `rumoro` and Authenticate, or to use the API-key option in this repository's README.

## Collect

Call `search_mentions` with `relevant: true`, `status: "open"`, `sort: "priority"`, `limit: 50` and `since` set to the start of yesterday in the user's timezone (ISO 8601). Add `platforms` or `keywordIds` if the user asked about a specific platform or keyword.

Also call `list_keywords` with `kind: ["competitor"]` (no `limit`, so all of them come back) and remember their `id`s: a mention whose `keyword.id` is in that list is about a competitor.

## Sort

Give every mention exactly one bucket, checking them in this order:

1. **Buying signals**: `classification.intents` includes `buy_intent`, or `churn_intent` on a competitor's keyword.
2. **Questions**: the intent `question`.
3. **Problems**: `bug_report` or `complaint`, or a review whose `review.rating` is 1 or 2.
4. **Other**: everything left.

Show each mention on one line: `post.platform`, the author (`author.name` or `author.handle`, plus `author.followers` if known), a one-sentence summary, and `post.url`. Add `classification.note` in brackets when it explains the score.

No results? Say so, and offer `get_mention_stats` for the past 7 days instead.

## Act

For each mention the user picks:

- **Reply**: write a draft that solves the person's problem first, says the user works on the product if it comes up, and sounds like the platform. When the project's `CLAUDE.md` sets reply rules or a voice, those win. Show it; don't post it.
- **Bug report**: if the post describes a bug in this codebase, find where it probably lives and draft a GitHub issue (title, steps to reproduce, link to the post).
- **Not relevant**: `update_mention` with `relevant: false`, so the correction is recorded.
- **Later**: `update_mention` with `snoozedUntil` set to when the user wants to see it again.

## Wrap up

Mark handled mentions with `update_mention` and `status: "done"`, and noise with `status: "ignored"`. Finish with a count: handled, ignored, still open.
