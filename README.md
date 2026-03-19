# FeedMansion for AI Agents

**Social media automation for AI agents, coding assistants, and automations.**

Connect FeedMansion to Claude, Cursor, Windsurf, ChatGPT, or any MCP-compatible AI tool to create content, schedule posts, and manage your social media automatically.

## What You Can Do

- **Create content with AI** — Generate posts, threads, and captions from URLs or prompts
- **Schedule automatically** — Let AI agents queue content for optimal posting times
- **Multi-platform posting** — One agent, multiple social networks (Twitter/X, Instagram, Facebook, LinkedIn, etc.)
- **Human oversight** — Agent tokens require approval before anything goes live

## Quick Start

1. **Get FeedMansion** — Sign up at [feedmansion.com](https://feedmansion.com)
2. **Generate a token** — Create a PAT at [feedmansion.com/settings/tokens](https://feedmansion.com/settings/tokens)
3. **Connect your AI tool** — Choose below:

| AI Tool | Difficulty | Setup |
|---------|------------|-------|
| [Claude Code / Claude Desktop](./platforms/claude/) | 🟢 Easy | [Guide](./platforms/claude/) |
| [Cursor IDE](./platforms/cursor/) | 🟢 Easy | [Guide](./platforms/cursor/) |
| [Windsurf](./platforms/windsurf/) | 🟢 Easy | [Guide](./platforms/windsurf/) |
| [OpenClaw](./platforms/openclaw/) | 🔴 Advanced | [Guide](./platforms/openclaw/) |

**Just getting started?** We recommend **Claude Code** — no server setup, works immediately.

<details>
<summary>🔧 Using another MCP-compatible tool?</summary>

See the [MCP setup guide](./mcp/README.md) for generic configuration that works with any MCP client.
</details>

## Example Usage

Once connected, ask your AI:

> "Create a LinkedIn post from this blog article and schedule it for tomorrow morning"

> "Generate a week's worth of tweets from my latest product update"

> "Show me my draft content and queue the approved ones"

> "What's my posting schedule looking like this week?"

## How It Works

FeedMansion exposes an **MCP (Model Context Protocol)** endpoint that gives AI agents access to:

- Your presences (brands/projects)
- Content drafts and posts
- Publishing queues and schedules
- Connected social accounts

```
AI Agent → MCP → FeedMansion → Social Networks
```

### Agent Approval Flow

For safety, agent tokens have restricted permissions:

| Action | What Happens |
|--------|--------------|
| Create draft | ✅ Done immediately |
| Edit draft | ✅ Done immediately |
| Submit for approval | ⏳ Moves to `pending_review` — you approve in web UI |
| Request scheduling | ⏳ Stored for review — you approve in web UI |

Nothing goes live without your sign-off.

## Available MCP Tools

| Tool | Description |
|------|-------------|
| `list_presences` | List your brands/projects |
| `get_presence` | Get presence with connected accounts |
| `list_content` | List content with status filter |
| `get_content` | Get specific content details |
| `create_content` | Create a new draft |
| `update_content` | Update an existing draft |
| `add_content_media` | Attach media from URL |
| `remove_content_media` | Remove attached media |
| `generate_content_from_url` | Generate AI content from a URL |
| `approve_content` | Submit for human approval |
| `enqueue_content` | Request to add to queue |

## Why FeedMansion?

- **Built for AI** — Native MCP support, designed for agent integration
- **Multi-platform** — Post to Twitter/X, Instagram, Facebook, LinkedIn, YouTube, TikTok, and more
- **Human in the loop** — Agent tokens can't post without approval
- **Ghost automations** — AI personas that match your brand voice
- **Smart scheduling** — Queue-based posting with timezone support

## Links

- [FeedMansion](https://feedmansion.com) — Sign up and start posting
- [Help Center](https://feedmansion.com/help/) — Documentation
- [MCP Setup Guide](https://feedmansion.com/help/mcp-setup/) — Detailed MCP instructions
- [GitHub](https://github.com/makerprism/feedmansion-ai-agents) — This repo

## Support

- Email: support@feedmansion.com
- [Help Center](https://feedmansion.com/help/)

## License

MIT
