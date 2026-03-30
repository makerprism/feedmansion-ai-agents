---
name: feedmansion
description: Manage FeedMansion content - drafts, queues, presences, and scheduling
metadata:
  openclaw:
    emoji: "✍️"
    category: content
    author: makerprism
    version: "2.0.0"
    minOpenClawVersion: "1.0.0"
---

# FeedMansion OpenClaw Skill

Control FeedMansion directly from OpenClaw! Create drafts, generate content from URLs, manage queues, and more.

## Prerequisites

1. A FeedMansion account with a Personal Access Token (PAT)
2. Generate a PAT at https://feedmansion.com/settings/tokens
3. Set the environment variable `FEEDMANSION_PAT` or configure in OpenClaw

## Configuration

Add to your OpenClaw config (`~/.openclaw/openclaw.json`):

```json
{
  "env": {
    "FEEDMANSION_PAT": "fm_pat_your_token_here"
  }
}
```

Or set the environment variable:
```bash
export FEEDMANSION_PAT="fm_pat_your_token_here"
```

## Tools

### feedmansion_list_presences

List all presences (brands/projects) in your FeedMansion account.

**Usage:**
```
Use feedmansion_list_presences to show me my presences
```

**Returns:** List of presences with their IDs, names, and connected accounts.

---

### feedmansion_create_draft

Create a new draft using AI generation in brand voice. This is the primary way to create content.

**Arguments:**
- `presence_id` (required): The presence/brand ID
- `topic` (required): What to write about
- `url` (optional): Article URL to generate from
- `media_urls` (optional): Array of image/video URLs to download and attach
- `ghost_id` (optional): Ghost writer ID for a specific writing style
- `custom_prompt` (optional): Custom writing instructions
- `title` (optional): Title for platforms like Reddit
- `queue_id` (optional): Queue to indicate publishing intent

**Usage:**
```
Create a draft for presence "abc123" about our new product launch
Generate a post from https://example.com/article for presence "abc123"
```

---

### feedmansion_batch_create_drafts

Create multiple drafts at once using AI. Up to 10 per call.

**Arguments:**
- `presence_id` (required): The presence/brand ID
- `items` (required): Array of objects with `topic` (required), `ghost_id` (optional), `custom_prompt` (optional)
- `queue_id` (optional): Queue intent for all items

---

### feedmansion_schedule_draft

Approve a draft and schedule it for publishing.

**Arguments:**
- `presence_id` (required): The presence/brand ID
- `draft_id` (required): The draft ID to schedule
- `target_account_ids` (required): Array of social account IDs to publish to
- `target_accounts` (optional): Array of objects with account_id and format preference
- `queue_id` (optional): Publishing queue to add this draft to
- `scheduled_for` (optional): ISO 8601 datetime to schedule for
- `publish_now` (optional): Set to true for immediate publishing

**Format options (for target_accounts):**
- `"auto"` - Auto-detect from media (default)
- `"post"` - Standard post (images, text)
- `"reel"` - Short-form vertical video
- `"story"` - Ephemeral story (Instagram only)

---

### feedmansion_edit_draft

Edit an existing draft's text or title.

**Arguments:**
- `presence_id` (required): The presence/brand ID
- `content_id` (required): The draft ID
- `content` (optional): New text
- `title` (optional): New title

---

### feedmansion_add_draft_media

Attach media to a draft from a URL or presigned upload.

**Arguments:**
- `presence_id` (required): The presence/brand ID
- `content_id` (required): The draft ID
- `media_url` (optional): URL to download media from
- `upload_id` (optional): Upload ID from create_media_upload_url

---

### feedmansion_list_drafts

List drafts for a presence.

**Arguments:**
- `presence_id` (required): The presence/brand ID
- `status` (optional): Filter by status (draft, approved, etc.)
- `limit` (optional): Max number of items to return

## MCP Endpoint

This skill connects to FeedMansion's MCP endpoint at:

```
https://feedmansion.com/mcp
```

## Error Handling

- `401 Unauthorized`: Check your PAT token is valid
- `403 Forbidden`: Token may lack required scopes
- `404 Not Found`: Presence or resource not found
- `429 Too Many Requests`: Rate limit exceeded, wait and retry

## Rate Limits

- 100 requests per minute per PAT
- 1000 AI generations per day (depends on plan)

## Support

- Documentation: https://feedmansion.com/en/help/
- MCP Setup Guide: https://feedmansion.com/en/help/mcp-setup/
- Support: support@feedmansion.com
