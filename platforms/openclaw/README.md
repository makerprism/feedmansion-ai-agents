# FeedMansion + OpenClaw

Use FeedMansion with OpenClaw for AI-powered social media management.

> **Self-hosted option** - OpenClaw runs on your own infrastructure, giving you full control and privacy. Best suited for users comfortable with server administration and security configuration.
>
> **Security note:** Properly securing an OpenClaw deployment (authentication, network exposure, token management, updates) is nontrivial. If misconfigured, it could expose your FeedMansion account or other connected services.
>
> For a simpler and more secure setup without self-hosting, try [Claude Code](../claude/), [Cursor](../cursor/), or [Windsurf](../windsurf/).

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
- "Create a draft about our new product launch"
- "Generate a post from this URL"
- "Show me my content queues"
- "Schedule these drafts for next week"

## Agent Tokens

For autonomous agents, create an **agent token** in FeedMansion. Agent tokens have restricted permissions:

- Can create and edit drafts
- Can schedule drafts (stored as intent, human approves in web UI)
- Cannot publish directly

This prevents agents from posting without oversight.
