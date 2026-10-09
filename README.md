# Rumoro skills for Claude Code

[Rumoro](https://rumoro.dev) tracks what people say about your product, your competitors and your topics on Reddit, X, Hacker News, GitHub, YouTube, LinkedIn and more, and scores every mention for relevance, sentiment and intent. These two skills let Claude Code set it up from your repository and go through new mentions with you.

## What you get

- **`/rumoro-setup`** reads the repository to learn what the product is, fills in your Rumoro company profile, proposes brand, competitor and topic keywords, and creates them once you agree.
- **`/rumoro-triage`** shows what people said since yesterday, grouped into buying intent, questions and bug reports, and drafts replies or GitHub issues for the ones you pick. Nothing is posted for you.

## Install

1. Connect the Rumoro MCP server, then sign in once in the browser:

   ```bash
   claude mcp add --transport http --scope user rumoro https://mcp.rumoro.dev/mcp
   ```

   In Claude Code, run `/mcp`, choose `rumoro` and Authenticate. New accounts start with $5.80 of free credit.

2. Copy the skills into your personal skills folder, so they work in every project:

   ```bash
   git clone https://github.com/rumoro-dev/claude-skills.git
   mkdir -p ~/.claude/skills
   cp -r claude-skills/skills/* ~/.claude/skills/
   ```

   To use them in one project only, copy them into that project's `.claude/skills/` instead.

3. Put your product, reply rules and voice in the project's `CLAUDE.md`. [`templates/CLAUDE.md`](templates/CLAUDE.md) shows the shape; fill in the placeholders. `/rumoro-triage` follows it when it drafts replies.

4. In Claude Code, run `/rumoro-setup`, and later `/rumoro-triage`.

### With an API key instead

For CI, a server, or if sign-in doesn't work, create a key on the API keys page at [app.rumoro.dev](https://app.rumoro.dev) and add the server with it:

```bash
claude mcp add --transport http rumoro https://mcp.rumoro.dev/mcp \
  --header "Authorization: Bearer $RUMORO_API_KEY"
```

## The rules the skills keep

Reply only where you'd answer without a product to sell. Answer first, say you work on the product when it comes up, and don't pitch. No direct messages, no duplicate replies. The skills find, sort and draft; you decide and post every reply yourself.

## Pricing

Prepaid, with no subscription. $5 per keyword per month plus $0.008 per matched mention. See [rumoro.dev/pricing](https://rumoro.dev/pricing).

## Links

- Docs: [docs.rumoro.dev](https://docs.rumoro.dev), MCP page at [docs.rumoro.dev/mcp](https://docs.rumoro.dev/mcp)
- Claude Code setup guide: [rumoro.dev/claude](https://rumoro.dev/claude)
- Support: markus@rumoro.dev

## License

These skills are released under the [MIT License](LICENSE). The Rumoro service itself is covered by its [terms](https://rumoro.dev/terms).
