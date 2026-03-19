# FeedMansion MCP Setup

FeedMansion exposes an MCP (Model Context Protocol) endpoint for AI agent integration.

## Endpoint

```
https://feedmansion.com/mcp
```

**Protocol Version:** `2025-03-26` (Streamable HTTP)

## Authentication

Use a Personal Access Token (PAT) with Bearer authentication:

```
Authorization: Bearer fm_pat_your_token_here
```

Generate a PAT at: [feedmansion.com/settings/tokens](https://feedmansion.com/settings/tokens)

## Transport

FeedMansion uses MCP Streamable HTTP transport:
- Single endpoint for all requests
- Session management via `Mcp-Session-Id` header
- Stateless or stateful operation

## Configuration Examples

### Claude Desktop / Claude Code

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

### Cursor

Add to your Cursor settings:

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

### OpenClaw

Add to `~/.openclaw/openclaw.json`:

```json
{
  "mcpServers": {
    "feedmansion": {
      "url": "https://feedmansion.com/mcp",
      "headers": {
        "Authorization": "Bearer ${FEEDMANSION_PAT}"
      }
    }
  },
  "env": {
    "FEEDMANSION_PAT": "fm_pat_your_token_here"
  }
}
```

### Windsurf

Add to your Windsurf MCP configuration.

## Session Management

The server returns a `Mcp-Session-Id` header on initialize. Include this in subsequent requests for stateful operation.

## Rate Limits

- 100 requests per minute per PAT
- AI generation limits depend on your FeedMansion plan

## Error Handling

| Status | Meaning |
|--------|---------|
| 401 | Invalid or missing token |
| 403 | Token lacks required permissions |
| 404 | Resource not found |
| 429 | Rate limit exceeded |

## Support

- [FeedMansion Help](https://feedmansion.com/help/)
- [MCP Specification](https://spec.modelcontextprotocol.io)
- Email: support@feedmansion.com
