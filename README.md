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

- **Create content with AI** — Generate posts, threads, and captions from topics or URLs
- **Attach images and videos** — Upload media via URLs or presigned uploads
- **Multi-platform posting** — One agent, multiple social networks (Twitter/X, Instagram, Facebook, LinkedIn, Bluesky, TikTok, YouTube, Threads, Mastodon, Pinterest, Reddit, Google Business Profile)
- **Human oversight** — Nothing goes live without your approval in the web UI

## Quick Start

1. **Get FeedMansion** — Sign up at [feedmansion.com](https://feedmansion.com)
2. **Generate a token** — Create a PAT at [feedmansion.com/settings/tokens](https://feedmansion.com/settings/tokens)
3. **Connect your AI tool** — Choose below:

| AI Tool | Difficulty | Setup |
|---------|------------|-------|
| [Claude Code / Claude Desktop](./platforms/claude/) | 🟢 Easy | [Guide](./platforms/claude/) |
| [Cursor IDE](./platforms/cursor/) | 🟢 Easy | [Guide](./platforms/cursor/) |
| [Windsurf](./platforms/windsurf/) | 🟢 Easy | [Guide](./platforms/windsurf/) |
| [OpenClaw](./platforms/openclaw/) | 🟡 Self-hosted | [Guide](./platforms/openclaw/) |

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

### Approval Flow

Nothing posts without your approval. When agents schedule content, it goes to a review queue in the FeedMansion web UI. You review and accept before anything goes live.

## Available MCP Tools (24 tools)

| Tool | Description |
|------|-------------|
| **Presences** | |
| `list_presences` | List all presences (brands/projects) |
| `get_presence` | Get presence details with accounts, queues |
| `update_presence` | Request changes to presence settings |
| `get_brand_voice` | Get full brand voice for external content |
| **Drafts** | |
| `list_drafts` | List drafts with optional status filter |
| `get_draft` | Get a specific draft with details, target accounts, and media |
| `create_draft` | Create draft from topic, URLs, source items, and/or images |
| `edit_draft` | Edit draft text or title (returns updated draft with media) |
| `batch_create_drafts` | Create up to 10 drafts in one call |
| **Media** | |
| `create_media_upload_url` | Get presigned URL for large file uploads |
| `add_draft_media` | Attach media (URL, upload_id, or base64) |
| `remove_draft_media` | Remove media from a draft |
| **Scheduling** | |
| `schedule_draft` | Approve + schedule (queue, specific time, or now) |
| `batch_schedule_drafts` | Approve + queue multiple drafts |
| `add_platforms_to_draft` | Add platforms to already-approved content |
| `batch_add_platforms_to_drafts` | Add platforms to multiple approved drafts (max 25) |
| `get_scheduled_and_queued_posts` | View scheduled and queued content |
| `get_published_posts` | View published and failed content |
| `list_queue_posts` | List all posts in a specific queue |
| `list_unscheduled_content` | List approved content with no queue/schedule assigned |
| **Accounts** | |
| `connect_account` | Connect a social account (Bluesky, Mastodon) |
| `list_unlinked_accounts` | List accounts not linked to a presence |
| `link_account_to_presence` | Link an account to a presence |
| **Feedback** | |
| `submit_feedback` | Submit a bug report, feature request, or general feedback |

### Key Workflows

#### Creating a Draft with AI

`create_draft` accepts multimodal input. Combine any of: topic text, URLs (up to 5), source items (feed/calendar/email), and media.

```json
{
  "name": "create_draft",
  "arguments": {
    "presence_id": "your-presence-id",
    "topic": "Announce our new product feature",
    "urls": ["https://example.com/blog/post"],
    "multi_draft": true
  }
}
```

Returns draft with `content_url` and `review_url`. When `multi_draft` is true, returns up to 5 draft variations. Always show draft text to user before scheduling.

#### Attaching Images

**Option 1: Public URL (easiest)**
```json
{
  "name": "create_draft",
  "arguments": {
    "presence_id": "...",
    "topic": "...",
    "media_urls": ["https://example.com/image.jpg"]
  }
}
```

**Option 2: Presigned URL (for local files)**
1. Create draft first
2. Call `create_media_upload_url` → get `upload_url` and `upload_id`
3. PUT the file to `upload_url` (binary, no base64)
4. Call `add_draft_media` with `upload_id`

> ⚠️ **Avoid base64 for images** — Shell ARG_MAX limits make inline base64 unreliable for files >10KB. Use presigned URLs instead.

#### Scheduling

```json
{
  "name": "schedule_draft",
  "arguments": {
    "presence_id": "...",
    "draft_id": "...",
    "target_account_ids": ["instagram-id", "twitter-id"],
    "publish_now": true
  }
}
```

Options: `queue_id`, `scheduled_for` (ISO 8601), or `publish_now`.

#### Adding Platforms to Existing Content

Already scheduled content to 3 platforms and need a 4th? No need to recreate anything:

```json
{
  "name": "add_platforms_to_draft",
  "arguments": {
    "presence_id": "...",
    "content_id": "...",
    "target_accounts": [
      {"account_id": "new-platform-id", "format": "auto"}
    ]
  }
}
```

For bulk operations (e.g. adding a platform to 50 scheduled drafts), use `batch_add_platforms_to_drafts` with up to 25 `content_ids` per call.

## Why FeedMansion?

- **Built for AI** — Native MCP support, designed for agent integration
- **Multi-platform** — 13 platforms supported
- **Human in the loop** — Agents can't post without approval
- **Brand voice** — AI generates content that matches your voice
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
