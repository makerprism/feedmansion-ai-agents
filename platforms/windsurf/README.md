# FeedMansion + Windsurf

Use FeedMansion with Windsurf IDE via MCP.

## Setup

1. Generate a PAT at [feedmansion.com/settings/tokens](https://feedmansion.com/settings/tokens)

2. Add to your Windsurf MCP configuration:

```json
{
  "mcpServers": {
    "feedmansion": {
      "url": "https://feedmansion.com/mcp",
      "headers": {
        "Authorization": "Bearer fm_pat_your_token_here"
      }
    }
  }
}
```

## Usage

Once configured, Windsurf's Cascade AI can:

- Create and manage social media content
- Generate posts from URLs
- Work with publishing queues
- Submit content for approval

## Agent Tokens

For autonomous agents, create an **agent token** in FeedMansion. Agent tokens restrict posting to require human approval in the web UI.
