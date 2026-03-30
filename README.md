# FeedMansion for AI Agents

**Social media automation for AI agents, coding assistants, and automations.**

Connect FeedMansion to Claude, Cursor, Windsurf, ChatGPT, or any MCP-compatible AI tool to create content, schedule posts, and manage your social media automatically.

<!--
⚠️ MAINTAINER NOTE: This repo documents FeedMansion's MCP tools.
   The canonical tool definitions are in:
   feedmansion.com repo → backend/lib/api/mcp_tools.ml

   When tools change, update:
   - This README.md (tool table)
   - platforms/openclaw/SKILL.md (detailed tool docs)
   - feedmansion.com/landing-pages/help/en/mcp-setup.md
-->

## What You Can Do

- **Create content with AI** - Generate posts, threads, and captions from topics or URLs
- **Schedule automatically** - Let AI agents queue content for optimal posting times
- **Multi-platform posting** - One agent, multiple social networks (Twitter/X, Instagram, Facebook, LinkedIn, etc.)
- **Human oversight** - Agent tokens require approval before anything goes live

## Quick Start

1. **Get FeedMansion** - Sign up at [feedmansion.com](https://feedmansion.com)
2. **Generate a token** - Create a PAT at [feedmansion.com/settings/tokens](https://feedmansion.com/settings/tokens)
3. **Connect your AI tool** - Choose below:

| AI Tool | Difficulty | Setup |
|---------|------------|-------|
| [Claude Code / Claude Desktop](./platforms/claude/) | 🟢 Easy | [Guide](./platforms/claude/) |
| [Cursor IDE](./platforms/cursor/) | 🟢 Easy | [Guide](./platforms/cursor/) |
| [Windsurf](./platforms/windsurf/) | 🟢 Easy | [Guide](./platforms/windsurf/) |
| [OpenClaw](./platforms/openclaw/) | 🟡 Self-hosted | [Guide](./platforms/openclaw/) |

**Just getting started?** We recommend **Claude Code** - no server setup, works immediately.

<details>
<summary>Using another MCP-compatible tool?</summary>

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
| Create draft | Done immediately |
| Edit draft | Done immediately |
| Schedule draft | Stored as intent. You approve in web UI |
| Publish | Not available to AI. Only humans can publish |

Nothing goes live without your sign-off.

## Available MCP Tools

**Discovery**

| Tool | Description |
|------|-------------|
| `list_presences` | List your brands/projects |
| `get_presence` | Get presence details with accounts, queues, and ghost writers |
| `list_drafts` | Browse drafts, optionally filtered by status |
| `get_draft` | Get specific draft details |
| `get_schedule` | View scheduled, queued, and published posts for a date range |
| `get_brand_voice` | Retrieve complete brand voice for external content creation |

**Create drafts**

| Tool | Description |
|------|-------------|
| `create_draft` | Create a draft using AI in brand voice. Optionally generate from a URL or attach media |
| `batch_create_drafts` | Create up to 10 drafts at once |

**Edit and media**

| Tool | Description |
|------|-------------|
| `edit_draft` | Edit an existing draft's text or title |
| `add_draft_media` | Attach an image or video to a draft |
| `remove_draft_media` | Remove attached media from a draft |
| `create_media_upload_url` | Get a presigned URL for large file uploads |

**Schedule**

| Tool | Description |
|------|-------------|
| `schedule_draft` | Approve a draft and schedule it (queue, specific time, or publish now) |
| `batch_schedule_drafts` | Schedule multiple drafts at once |

**Settings and feedback**

| Tool | Description |
|------|-------------|
| `update_presence` | Request changes to presence settings |
| `submit_bug_report` | Report a bug |
| `submit_feature_request` | Suggest a feature |

## Why FeedMansion?

- **Built for AI** - Native MCP support, designed for agent integration
- **Multi-platform** - Post to Twitter/X, Instagram, Facebook, LinkedIn, YouTube, TikTok, and more
- **Human in the loop** - Agent tokens can't post without approval
- **Brand voice** - AI writes in your brand's voice automatically
- **Smart scheduling** - Queue-based posting with timezone support

## Links

- [FeedMansion](https://feedmansion.com) - Sign up and start posting
- [Help Center](https://feedmansion.com/help/) - Documentation
- [MCP Setup Guide](https://feedmansion.com/help/mcp-setup/) - Detailed MCP instructions

## Support

- Email: support@feedmansion.com
- [Help Center](https://feedmansion.com/help/)

## License

MIT
