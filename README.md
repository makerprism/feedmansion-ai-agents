# FeedMansion AI Agents

Use FeedMansion with your favorite AI coding assistant or agent.

FeedMansion exposes an [MCP (Model Context Protocol)](https://modelcontextprotocol.io) endpoint that lets AI agents create content, manage queues, and schedule posts on your behalf.

## What's Inside

- **[MCP Setup Guide](./mcp/README.md)** — Connect FeedMansion to any MCP-compatible tool
- **[Platform Guides](./platforms/)** — Ready-to-use skills and prompts for:
  - [Claude Code](./platforms/claude/)
  - [Cursor](./platforms/cursor/)
  - [Windsurf](./platforms/windsurf/)
  - [OpenClaw](./platforms/openclaw/)
  - [More coming soon...](./platforms/README.md)

## Quick Start

1. Generate a Personal Access Token at [feedmansion.com/settings/tokens](https://feedmansion.com/settings/tokens)
2. Choose your platform from the [platforms folder](./platforms/)
3. Follow the setup instructions

## MCP Endpoint

```
https://feedmansion.com/mcp
```

Authentication: Bearer token (your PAT)

## Available Tools

| Tool | Description |
|------|-------------|
| `list_presences` | List your presences (brands/projects) |
| `get_presence` | Get presence details with social accounts |
| `list_content` | List content items with optional status filter |
| `get_content` | Get specific content details |
| `create_content` | Create a new content draft |
| `update_content` | Update an existing draft |
| `add_content_media` | Attach media from URL to a draft |
| `remove_content_media` | Remove media from a draft |
| `generate_content_from_url` | Generate AI content from a URL |
| `approve_content` | Submit content for approval |
| `enqueue_content` | Request to add content to a queue |

## Agent Approval Flow

When using agent tokens, some actions require human approval:

| Action | Agent Behavior |
|--------|---------------|
| `approve_content` | Moves to `pending_review` — human approves in web UI |
| `enqueue_content` | Stores enqueue request — human approves in web UI |

After these actions, the agent receives a `review_url` to share with you.

## Support

- [FeedMansion Help](https://feedmansion.com/help/)
- [MCP Setup Guide](https://feedmansion.com/help/mcp-setup/)
- Email: support@feedmansion.com

## License

MIT
