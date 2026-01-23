---
title: WhatsApp Cloud API via Kapso Proxy
---

# WhatsApp Cloud API (Kapso Meta Proxy)

REST API reference for sending messages and querying history via Kapso's Meta proxy.

## Base URL and auth

```
Base URL: ${KAPSO_API_BASE_URL}/meta/whatsapp/v24.0
Auth header: X-API-Key: <api_key>
```

All payloads mirror the Meta Cloud API. Kapso adds storage and query features.

## Send messages

Endpoint: `POST /{phone_number_id}/messages`

All payloads require `messaging_product: "whatsapp"`.

### Text

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "text",
  "text": { "body": "Hello!" }
}
```

### Image

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "image",
  "image": { "link": "https://example.com/photo.jpg", "caption": "Photo" }
}
```

Use `id` instead of `link` for uploaded media.

### Video

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "video",
  "video": { "link": "https://example.com/clip.mp4", "caption": "Video" }
}
```

### Audio

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "audio",
  "audio": { "link": "https://example.com/audio.mp3" }
}
```

### Document

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "document",
  "document": { "link": "https://example.com/file.pdf", "filename": "file.pdf", "caption": "Report" }
}
```

### Sticker

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "sticker",
  "sticker": { "id": "<MEDIA_ID>" }
}
```

### Location

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "location",
  "location": { "latitude": -33.45, "longitude": -70.66, "name": "Santiago", "address": "CL" }
}
```

### Contacts

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "contacts",
  "contacts": [
    { "name": { "formatted_name": "John Doe" }, "phones": [{ "phone": "+15551234567", "type": "WORK" }] }
  ]
}
```

### Reaction

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "reaction",
  "reaction": { "message_id": "wamid......", "emoji": "👍" }
}
```

## Interactive messages

Require an active 24-hour session window. Use templates for outbound notifications outside the window.

### Buttons

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "interactive",
  "interactive": {
    "type": "button",
    "body": { "text": "Choose an option" },
    "action": {
      "buttons": [
        { "type": "reply", "reply": { "id": "yes", "title": "Yes" } },
        { "type": "reply", "reply": { "id": "no", "title": "No" } }
      ]
    }
  }
}
```

### List

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "interactive",
  "interactive": {
    "type": "list",
    "body": { "text": "Choose an option" },
    "action": {
      "button": "View options",
      "sections": [
        {
          "title": "Menu",
          "rows": [
            { "id": "opt_1", "title": "Option 1", "description": "First option" },
            { "id": "opt_2", "title": "Option 2", "description": "Second option" }
          ]
        }
      ]
    }
  }
}
```

### CTA URL

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "interactive",
  "interactive": {
    "type": "cta_url",
    "body": { "text": "Track your order" },
    "action": {
      "name": "cta_url",
      "parameters": { "display_text": "Track order", "url": "https://example.com/orders/123" }
    }
  }
}
```

### Location request

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "interactive",
  "interactive": {
    "type": "location_request_message",
    "body": { "text": "Please share your location." },
    "action": { "name": "send_location" }
  }
}
```

### Catalog message

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "interactive",
  "interactive": {
    "type": "catalog_message",
    "body": { "text": "Browse our catalog." },
    "action": {
      "name": "catalog_message",
      "parameters": { "thumbnail_product_retailer_id": "SKU_THUMBNAIL" }
    }
  }
}
```

## Template messages

```json
{
  "messaging_product": "whatsapp",
  "to": "15551234567",
  "type": "template",
  "template": {
    "name": "order_ready_named",
    "language": { "code": "en_US" },
    "components": [
      {
        "type": "body",
        "parameters": [
          { "type": "text", "parameter_name": "order_id", "text": "ORDER-123" }
        ]
      }
    ]
  }
}
```

See [templates-reference.md](templates-reference.md) for full component rules.

## Mark as read

```json
{
  "messaging_product": "whatsapp",
  "status": "read",
  "message_id": "wamid......"
}
```

## Query history (Kapso)

These endpoints are Kapso-specific and store/retrieve conversation data.

### List messages

```
GET /{phone_number_id}/messages
```

| Param | Description |
|-------|-------------|
| `conversation_id` | Filter by conversation UUID |
| `direction` | `inbound` or `outbound` |
| `status` | `pending`, `sent`, `delivered`, `read`, `failed` |
| `since` / `until` | ISO 8601 timestamps |
| `limit` | Max 100 |
| `before` / `after` | Cursor pagination |
| `fields` | Use `kapso(...)` for extra fields |

### List conversations

```
GET /{phone_number_id}/conversations
```

| Param | Description |
|-------|-------------|
| `status` | `active` or `ended` |
| `last_active_since` / `last_active_until` | ISO 8601 timestamps |
| `phone_number` | Filter by customer phone (E.164) |
| `limit` | Max 100 |
| `before` / `after` | Cursor pagination |
| `fields` | Use `kapso(...)` for extra fields |

### Get a conversation

```
GET /{phone_number_id}/conversations/{conversation_id}
```

## Kapso extensions

Add `fields=kapso(...)` to list endpoints to include extra data:

- `kapso(default)` or `kapso(*)` - all default fields
- `kapso(direction,media_url,contact_name)` - specific fields
- `kapso()` - omit Kapso fields entirely

Common fields: `direction`, `status`, `media_url`, `contact_name`, `flow_response`, `flow_token`.

See SDK docs for the full field list.

## Media upload

```
POST /{phone_number_id}/media
```

Returns `id` for use in send payloads.

## Notes

- Discover `phone_number_id` + `business_account_id` via `GET /platform/v1/whatsapp/phone_numbers`
- All send payloads require `messaging_product: "whatsapp"`
- Graph version is controlled by `KAPSO_META_GRAPH_VERSION` (default `v24.0`)
