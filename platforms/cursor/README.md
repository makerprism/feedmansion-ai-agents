# FeedMansion + Cursor

Use FeedMansion with Cursor IDE via MCP.

## Setup

1. Generate a PAT at [feedmansion.com/settings/tokens](https://feedmansion.com/settings/tokens)

2. Add to your Cursor MCP settings:

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

Once configured, Cursor's AI assistant can:

- Create social media content drafts
- Generate posts from URLs
- List and manage your content queues
- Submit content for approval

## Agent Tokens

For autonomous use, create an **agent token** in FeedMansion. This ensures all posts require human approval before going live.
