# FeedMansion + OpenClaw

Use FeedMansion with OpenClaw for AI-powered social media management.

> ⚠️ **Not recommended for non-technical users**
>
> OpenClaw requires self-hosting, server administration, and manual configuration. If you're looking for a simpler experience, we recommend:
> - **[Claude Code](../claude/)** — Install and go, no server needed
> - **[Cursor](../cursor/)** — Built into your IDE
> - **[Windsurf](../windsurf/)** — Simple MCP setup
>
> OpenClaw is powerful but requires comfort with Linux, Docker, and security configuration.

## Setup

1. Generate a PAT at [feedmansion.com/settings/tokens](https://feedmansion.com/settings/tokens)

2. Add to `~/.openclaw/openclaw.json`:

```json
{
  "env": {
    "FEEDMANSION_PAT": "fm_pat_your_token_here"
  }
}
```

3. Create the skill file at `~/.openclaw/workspace/skills/feedmansion/SKILL.md`:

See [SKILL.md](./SKILL.md) for the full skill definition.

## Usage

Once configured, you can ask OpenClaw to:

- "List my FeedMansion presences"
- "Create a draft for presence X with text..."
- "Generate a post from this URL..."
- "Show me my content queues"
- "Enqueue this content for scheduling"

## Agent Tokens

For autonomous agents, create an **agent token** in FeedMansion. Agent tokens have restricted permissions:

- Can create and edit drafts
- Can submit content for approval (moves to `pending_review`)
- Can request enqueue (human must approve in web UI)

This prevents agents from posting without oversight.
