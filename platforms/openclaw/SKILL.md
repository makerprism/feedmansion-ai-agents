---
name: feedmansion
description: Manage FeedMansion content - drafts, queues, presences, and ghost automations
metadata:
  openclaw:
    emoji: "✍️"
    category: content
    author: makerprism
    version: "3.0.0"
    minOpenClawVersion: "1.0.0"
    source: https://github.com/makerprism/feedmansion-ai-agents
---

# FeedMansion OpenClaw Skill

Control FeedMansion directly from OpenClaw! Create drafts, generate content from URLs, attach media, and schedule posts.

> **Canonical source:** [github.com/makerprism/feedmansion-ai-agents](https://github.com/makerprism/feedmansion-ai-agents)

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

---

## ⚠️ Recommended Workflow

**Always follow this sequence to avoid mistakes:**

1. **Start with:** `list_presences` → `get_presence(presence_id)`
2. **Show user the platforms:** "Your accounts: X (Twitter), Y (Bluesky), Z (Mastodon)"
3. **Ask which to use** before any create/schedule operations
4. **After creating drafts:** Show text to user for review
5. **After scheduling:** Verify with `get_scheduled_and_queued_posts` or check queue count

> **Never assume platforms.** Always confirm with user which accounts to post to.

---

## Tools Overview (24 tools)

| Tool | Description |
|------|-------------|
| **Presences** | |
| `list_presences` | List all presences you have access to |
| `get_presence` | Get presence details, accounts, queues |
| `update_presence` | Request changes to presence settings |
| `get_brand_voice` | Get full brand voice for external content creation |
| **Drafts** | |
| `list_drafts` | List drafts with optional status filter |
| `get_draft` | Get a specific draft with details, target accounts, and media |
| `create_draft` | Create draft from topic, URLs, source items, and/or images |
| `edit_draft` | Edit draft text or title (returns updated draft with media) |
| `batch_create_drafts` | Create up to 10 drafts in one call |
| **Media** | |
| `create_media_upload_url` | Get presigned URL for large file uploads |
| `add_draft_media` | Attach media to a draft |
| `remove_draft_media` | Remove media from a draft |
| **Scheduling** | |
| `schedule_draft` | Approve + schedule a single draft |
| `batch_schedule_drafts` | Approve + queue multiple drafts (max 25) |
| `add_platforms_to_draft` | Add platforms to already-approved content |
| `batch_add_platforms_to_drafts` | Add platforms to multiple approved drafts (max 25) |
| `get_scheduled_and_queued_posts` | View scheduled and queued content |
| `get_published_posts` | View published and failed content |
| **Accounts** | |
| `connect_account` | Connect a social account (Bluesky, Mastodon) |
| `list_unlinked_accounts` | List accounts not linked to a presence |
| `link_account_to_presence` | Link an account to a presence |
| **Feedback** | |
| `submit_feedback` | Submit a bug report, feature request, or general feedback |

---

## Presence Tools

### list_presences

List all presences (brands/projects) you have access to.

**Returns:** List with id, name, who_you_are, posting_language, timezone, account_count, member_count, presence_url.

---

### get_presence

Get full details about a presence including connected social accounts, publishing queues.

**Arguments:**
- `presence_id` (required): The presence ID

**Returns:** Presence details + `accounts[]` + `queues[]`

**Key fields:**
- `accounts[].id`, `accounts[].platform`, `accounts[].platform_username` — for scheduling
- `queues[].id`, `queues[].name`, `queues[].schedule`, `queues[].queued_count`

---

### update_presence

Request changes to presence settings. Creates a change request for review.

**Arguments:**
- `presence_id` (required)
- `name` (optional)
- `who_you_are`, `what_you_do`, `why_you_do_it` (optional)
- `posting_language` (optional): e.g. 'en', 'de'
- `timezone` (optional): IANA timezone
- `brand_voice_description`, `brand_voice_examples` (optional)
- `auto_generate_alt_text` (optional): boolean

**Returns:** `change_request_id`, `review_url`

---

### get_brand_voice

Retrieve the complete brand voice for creating content externally.

**Arguments:**
- `presence_id` (required)

**Returns:** Full brand voice including voice_dimensions, voice_profile, etc.

**Use when:** Creating visual content outside FeedMansion and need to match brand voice for captions.

---

## Draft Tools

### list_drafts

List drafts for a presence with optional filtering.

**Arguments:**
- `presence_id` (required)
- `status` (optional): `draft`, `approved`, `rejected`, `published`, `partially_published`
- `limit` (optional): 1-100 (default 50)
- `offset` (optional): default 0

**Returns:** `{ items: [], total, limit, offset }`

---

### get_draft

Get a specific draft with its details, target accounts, and attached media.

**Arguments:**
- `presence_id` (required)
- `content_id` (required)

**Returns:** Draft with all fields, `target_accounts[]`, and `media[]` (each with presigned `storage_url`).

---

### create_draft

Create a draft with AI-generated content. Supports multimodal input: combine text, URLs, source items, and media. At least one of `topic`, `url`, `urls`, or `source_item_id` is required.

**Arguments:**
- `presence_id` (required)
- `topic` (optional): What to write about, or freeform text/notes for generation
- `url` (optional): Single article URL to generate from
- `urls` (optional): Array of up to 5 URLs to generate from (content fetched and combined)
- `source_item_id` (optional): ID of a feed item, calendar event, or email inbox item
- `source_item_type` (optional, required with source_item_id): `"feed_item"`, `"calendar_event"`, or `"email_inbox_item"`
- `multi_draft` (optional, default false): When true, generates up to 5 draft variations
- `media_urls` (optional): Array of public image/video URLs to attach
- `media` (optional): Array of media objects with `url` or `base64_data` + `filename`
- `ghost_id` (optional): Ghost writer ID for specific style (see get_presence)
- `custom_prompt` (optional): Custom writing instructions (mutually exclusive with ghost_id)
- `title` (optional): For platforms like Reddit
- `queue_id` (optional): Queue intent

**Returns:** Single draft: `content_url`, `review_url`, draft content. Multi-draft (`multi_draft: true`): `draft_count`, `draft_ids[]`, `first_draft`, `content_url`, `review_url`.

**After creating:** Always show the draft text to user for review before scheduling.

> ⚠️ **Image uploads:** Use `media_urls` for public URLs. For local files, use the presigned URL workflow (see Media Tools below). Avoid `base64_data` for images >10KB due to shell ARG_MAX limits.

---

### edit_draft

Edit an existing draft's text or title. Only works on drafts (not approved/published).

**Arguments:**
- `presence_id` (required)
- `content_id` (required)
- `content` (optional): New text
- `title` (optional): New title (pass empty string to clear)

**Returns:** Updated draft with `target_accounts[]` and `media[]` (each with presigned `storage_url`).

---

### batch_create_drafts

Create multiple drafts in one call (max 10).

**Arguments:**
- `presence_id` (required)
- `items` (required): Array of 1-10 items, each with:
  - `topic` (required)
  - `ghost_id` OR `custom_prompt` (optional)
- `queue_id` (optional): Applied to all items

**Returns:** `created`, `total`, `review_url`, `results[]`

---

## Media Tools

> **Recommended workflow for images:** Use public URLs (`media_urls` on create_draft) when possible. For local files, use the presigned URL workflow below.

### create_media_upload_url

Get a presigned URL for uploading large files directly to storage.

**Arguments:**
- `presence_id` (required)
- `content_id` (required): Must be a draft
- `filename` (required): e.g. 'photo.jpg', 'clip.mp4'

**Returns:** `upload_url` (PUT), `upload_id`, `content_type`, `expires_in_seconds`

---

### add_draft_media

Attach media to a draft. Provide ONE of: `media_url`, `upload_id`, or `base64_data` + `filename`.

**Arguments:**
- `presence_id` (required)
- `content_id` (required)
- `media_url` (optional): Download from public URL
- `upload_id` (optional): From create_media_upload_url
- `base64_data` + `filename` (optional): Base64-encoded file (avoid for images >10KB)
- `alt_text` (optional)

**Supported types:** jpg, png, gif, webp, mp4, mov, webm (max 50MB)

---

### remove_draft_media

Remove attached media from a draft.

**Arguments:**
- `presence_id` (required)
- `content_id` (required)
- `media_id` (required)

---

### Presigned URL Upload Workflow

For local images that don't have public URLs:

1. **Create draft first** (without media)
2. **Get upload URL:**
   ```
   create_media_upload_url(presence_id, content_id, filename)
   → returns upload_url, upload_id
   ```
3. **Upload file:** PUT the binary file to `upload_url` with `Content-Type` header
4. **Attach to draft:**
   ```
   add_draft_media(presence_id, content_id, upload_id=upload_id)
   ```

---

## Scheduling Tools

### schedule_draft

Approve a draft and schedule it in one step.

**Arguments:**
- `presence_id` (required)
- `draft_id` (required)
- `target_account_ids` OR `target_accounts` (required)
- ONE of: `queue_id`, `scheduled_for`, or `publish_now`

**target_accounts format (for per-account format control):**
```json
[
  {"account_id": "instagram-id", "format": "reel"},
  {"account_id": "facebook-id", "format": "post"}
]
```

**Format options:** `auto` (default), `post`, `reel`, `story`

**Scheduling modes:**
- `queue_id`: Add to queue's schedule
- `scheduled_for`: ISO 8601 datetime for specific time
- `publish_now`: Request immediate publishing

**Returns:** `status: "approved"`, `post_ids[]`, `content_url`

**Always share the review_url with the user.**

---

### batch_schedule_drafts

Approve and queue multiple drafts (max 25). Queue scheduling only.

> ⚠️ **REQUIRED:** You MUST specify `target_account_ids` or `target_accounts`. There is no default. Always confirm with user which platforms to use.

**Arguments:**
- `presence_id` (required)
- `draft_ids` (required): Array of 1-25 draft IDs
- `target_account_ids` OR `target_accounts` (required) — list from `get_presence`
- `queue_id` (required)

**Returns:** `approved`, `total`, `results[]`

**After scheduling:** Verify posts were created by checking `get_scheduled_and_queued_posts` or queue `queued_count`.

---

### add_platforms_to_draft

Add new social accounts to an already-approved draft. Generates platform-specific posts for the new accounts while preserving all existing posts and scheduling.

**Arguments:**
- `presence_id` (required)
- `content_id` (required): Must be in Approved status
- `target_accounts` (required): Array of `{account_id, format?}` - must not overlap with existing targets

**Returns:** `status`, `new_post_count`, `new_post_ids[]`, `content_url`

**Use when:** Content is already approved or scheduled and you need to publish to additional accounts without recreating drafts.

---

### batch_add_platforms_to_drafts

Add new social accounts to multiple already-approved drafts (max 25). Same target accounts are added to all drafts.

**Arguments:**
- `presence_id` (required)
- `content_ids` (required): Array of 1-25 content IDs (all must be Approved)
- `target_accounts` (required): Array of `{account_id, format?}`

**Returns:** `status`, `message`, `results[]` (per-item success/error)

**Use when:** Adding a new platform to many scheduled posts at once (e.g. adding a 4th platform to 50 drafts).

---

### get_scheduled_and_queued_posts

View scheduled and queued content that has not yet been published.

**Arguments:**
- `presence_id` (required)
- `from` (optional): ISO 8601 datetime (defaults to now)
- `to` (optional): ISO 8601 datetime (defaults to 14 days from now)
- `limit` (optional): default 100

**Use for:** Seeing what's coming up, finding free time slots, checking for scheduling conflicts.

---

### get_published_posts

View content that has been published, partially published, or has failed posts.

**Arguments:**
- `presence_id` (required)
- `from` (optional): ISO 8601 datetime (defaults to 14 days ago)
- `to` (optional): ISO 8601 datetime (defaults to now)
- `limit` (optional): default 100

**Use for:** Reviewing what went out, checking publishing results, identifying failed posts. Results ordered most recent first.

---

## Account Tools

### connect_account

Connect a social account. Supports Bluesky and Mastodon (others require OAuth in web UI).

**Arguments:**
- `presence_id` (required)
- `platform` (required): `bluesky`, `mastodon`, etc.
- `identifier` (Bluesky): handle or email
- `app_password` (Bluesky): app password from bsky.app
- `instance_url` (Mastodon): instance URL

---

### list_unlinked_accounts

List accounts not linked to any presence.

---

### link_account_to_presence

Link an existing account to a presence.

**Arguments:**
- `presence_id` (required)
- `account_id` (required)

---

## Feedback Tools

### submit_feedback

Submit feedback: a bug report, feature request, or general feedback (thank you notes, comments, suggestions).

**Arguments:**
- `feedback_type` (required): `bug_report`, `feature_request`, or `general`
- `description` (required): The feedback message
- `title` (optional): Short title (max 200 chars, required for feature requests)
- `steps_to_reproduce` (optional): Only relevant for bug reports

---

## MCP Endpoint

This skill connects to FeedMansion's MCP endpoint:

```
https://feedmansion.com/mcp
```

## Error Handling

- `401 Unauthorized`: Check your PAT token
- `403 Forbidden`: Token lacks required scopes
- `404 Not Found`: Presence or resource not found
- `429 Too Many Requests`: Rate limit exceeded

## Rate Limits

- 100 requests per minute per PAT
- AI generations: rate limited per presence

## Support

- Documentation: https://feedmansion.com/en/help/
- MCP Setup Guide: https://feedmansion.com/en/help/mcp-setup/
- Support: support@feedmansion.com
