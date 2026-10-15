# Rumoro skills for Claude Code

[Rumoro](https://rumoro.dev) tracks what people say about your product, your competitors and your topics on Reddit, X, Hacker News, GitHub, YouTube, LinkedIn and more. It scores every mention for relevance, sentiment and intent. With these two skills, Claude Code can set Rumoro up from your repository and go through new mentions with you.

## What the skills do

- **`/rumoro-setup`** learns what your product is from the repository, fills in your Rumoro company profile, suggests keywords for your brand, your competitors and your topics, then creates the ones you approve.
- **`/rumoro-triage`** shows what people said since yesterday, sorted into buying signals, questions and problems. It drafts replies or GitHub issues for the posts you choose and never posts for you.

## Install

1. Add the Rumoro MCP server to Claude Code.

   ```bash
   claude mcp add --transport http --scope user rumoro https://mcp.rumoro.dev/mcp
   ```

   Then run `/mcp` in Claude Code, choose `rumoro` and Authenticate in the browser. New accounts start with $5.80 of free credit.

2. Copy the skills into your personal skills folder so they work in every project.

   ```bash
   git clone https://github.com/rumoro-dev/claude-skills.git
   mkdir -p ~/.claude/skills
   cp -r claude-skills/skills/* ~/.claude/skills/
   ```

   For a single project, copy them into that project's `.claude/skills/` instead.

3. Add the section in [`templates/CLAUDE.md`](templates/CLAUDE.md) to your project's `CLAUDE.md` and fill in the placeholders for your product, reply rules and voice. `/rumoro-triage` follows it when it drafts replies.

4. Run `/rumoro-setup` once. After that, run `/rumoro-triage` whenever you want to go through new mentions.

### With an API key instead

For CI, a server, or when sign-in doesn't work, create a key on the API keys page at [app.rumoro.dev](https://app.rumoro.dev) and add the server with it.

```bash
claude mcp add --transport http rumoro https://mcp.rumoro.dev/mcp \
  --header "Authorization: Bearer $RUMORO_API_KEY"
```

## What stays with you

The skills look for posts, group them and write drafts. You choose what to answer and publish each reply yourself. A draft answers the person's question first and brings up your product only when it's the real answer, saying that you work on it. The skills never post, never send direct messages and never paste the same reply into two threads.

## Pricing

Prepaid, with no subscription. Each keyword costs $5 per month, and each matched mention $0.008. See [rumoro.dev/pricing](https://rumoro.dev/pricing).

## Links

- [Documentation](https://docs.rumoro.dev)
- [MCP guide](https://docs.rumoro.dev/mcp)
- [Claude Code setup guide](https://rumoro.dev/claude)
- Example prompts by platform: the [Reddit](https://rumoro.dev/reddit-mcp), [X](https://rumoro.dev/x-mcp), [LinkedIn](https://rumoro.dev/linkedin-mcp), [YouTube](https://rumoro.dev/youtube-mcp), [TikTok](https://rumoro.dev/tiktok-mcp) and [Instagram](https://rumoro.dev/instagram-mcp) MCP server pages
- Support at markus@rumoro.dev

## License

These skills are released under the [MIT License](LICENSE). Use of the Rumoro service is covered by its [terms](https://rumoro.dev/terms).
