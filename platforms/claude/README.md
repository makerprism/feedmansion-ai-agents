# FeedMansion + Claude

Use FeedMansion with Claude Code or Claude Desktop via MCP.

## Claude Code

Add to your Claude Code configuration (`~/.claude/claude_desktop_config.json` or project-level):

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

## Claude Desktop

Same configuration as Claude Code.

## Usage

Once configured, Claude will have access to FeedMansion tools:

- List and manage presences
- Create and edit content drafts
- Generate AI content from URLs
- Submit content for approval
- Manage publishing queues

## Agent Tokens

For autonomous use, create an **agent token** in FeedMansion. This restricts posting actions to require human approval.

## Get a Token

Generate a PAT at: [feedmansion.com/settings/tokens](https://feedmansion.com/settings/tokens)
