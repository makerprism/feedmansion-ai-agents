---
name: feedmansion
description: Manage FeedMansion content - drafts, queues, presences, and ghost automations
metadata:
  openclaw:
    emoji: "✍️"
    category: content
    author: makerprism
    version: "1.2.0"
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

### feedmansion_create_content

Create a new draft in a specific presence.

**Arguments:**
- `presence_id` (required): The presence/brand ID
- `text` (required): The content text
- `target_account_ids` (optional): Array of social account IDs to target

**Usage:**
```
Create a draft for presence "abc123" with text "Hello world!"
```

---

### feedmansion_generate_from_url

Generate AI content from a URL source.

**Arguments:**
- `url` (required): The URL to generate content from
- `presence_id` (required): The presence/brand ID
- `tone` (optional): Tone for the content (professional, casual, etc.)

**Usage:**
```
Generate a post from https://example.com/article for presence "abc123"
```

---

### feedmansion_list_queues

List all content queues for a presence.

**Arguments:**
- `presence_id` (required): The presence/brand ID

---

### feedmansion_list_content

List drafts/content for a presence.

**Arguments:**
- `presence_id` (required): The presence/brand ID
- `status` (optional): Filter by status (draft, pending_review, approved, etc.)
- `limit` (optional): Max number of items to return

---

### feedmansion_approve_content

Submit a draft for review. With agent tokens, content moves to `pending_review` for human approval.

**Arguments:**
- `content_id` (required): The content ID to approve
- `target_account_ids` (optional): Array of social account IDs to publish to
- `target_accounts` (optional): Array of objects with account_id and format preference

**Format options:**
- `"auto"` - Auto-detect from media (default)
- `"post"` - Standard post (images, text)
- `"reel"` - Short-form vertical video
- `"story"` - Ephemeral story (Instagram only)

**Usage:**
```
# Simple: just account IDs (format = auto)
Approve content "xyz789" for accounts ["acc1", "acc2"]

# With format control per account
Approve content "xyz789" for:
- account "acc1" as reel
- account "acc2" as post
```

**Example with format:**
```json
{
  "content_id": "xyz789",
  "target_accounts": [
    {"account_id": "instagram-account-id", "format": "reel"},
    {"account_id": "facebook-account-id", "format": "post"}
  ]
}
```

---

### feedmansion_enqueue_content

Request to add content to a publishing queue.

**Arguments:**
- `content_id` (required): The content ID
- `queue_id` (required): The queue ID to add content to

**Behavior:**
- With agent tokens: stores the enqueue request for human approval
- Returns `review_url` where human can approve/reject
- Human sees the request in FeedMansion web UI → approves → content gets scheduled

**Usage:**
```
Enqueue content "xyz789" into queue "queue-abc"
```

**Response:**
```json
{
  "status": "pending_enqueue",
  "message": "Enqueue request saved. A team member will review and schedule in the web UI.",
  "queue_id": "queue-abc",
  "review_url": "https://feedmansion.com/app/p/presence-id/review"
}
```

---

### feedmansion_schedule_content

Schedule content for a specific time.

**Arguments:**
- `content_id` (required): The content ID
- `scheduled_at` (required): ISO 8601 datetime string

---

## Agent Approval Flow

When using an agent token, certain actions require human approval before execution:

| Action | Agent Behavior | Human Action |
|--------|---------------|--------------|
| `approve_content` | Moves to `pending_review` status | Review in web UI, then approve |
| `enqueue_content` | Stores `requested_queue_id` | Review in web UI, approve or reject |

**Best practices:**
- After calling approve/enqueue, inform the human with the `review_url`
- Example: "I've submitted 3 posts for your approval. Review them here: [link]"
- Human clicks link → sees pending items → approves/rejects in FeedMansion UI

---

### feedmansion_list_ghosts

List all ghost automations for a presence.

**Arguments:**
- `presence_id` (required): The presence/brand ID

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
