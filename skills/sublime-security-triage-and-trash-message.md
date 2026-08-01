---
name: Triage and trash a malicious message
description: Search flagged message groups, inspect a message, and trash it (with restore as the undo) across a Sublime deployment.
api: openapi/sublime-security-platform-openapi.json
operations: [searchMessageGroups, getMessageGroup, getMessage, trashMessageCanonicalGroup, restoreMessageCanonicalGroup, actionMessage]
---

# Triage and trash a malicious message

Use the Sublime Platform API to hunt for a flagged email, review it, and remediate.

## Auth
Send `Authorization: Bearer <API key>` on every request (see `authentication/sublime-security-authentication.yml`). Base URL is your deployment's region host, e.g. `https://platform.sublime.security`. Log the `X-Request-ID` response header.

## Steps
1. **Find candidates** — `searchMessageGroups` (`GET /v0/message-groups/search`) with filters (e.g. `flagged`, `flagged_rule_id__is`, `created_at[gte]`). Page with `cursor` + `limit`.
2. **Inspect the group** — `getMessageGroup` (`GET /v0/message-groups/{id}`) to see the flagged rules and member messages; drill into a single message with `getMessage` (`GET /v0/messages/{id}`).
3. **Remediate the whole group** — `trashMessageCanonicalGroup` (`POST /v0/message-groups/{id}/trash`) to trash every message in the canonical group.
4. **Undo if needed** — `restoreMessageCanonicalGroup` (`POST /v0/message-groups/{id}/restore`).
5. **Single-message action (alternative)** — `actionMessage` (`POST /v0/messages/{id}/actions`) to apply an action to one message.

## Notes
- Actions are asynchronous; poll state where offered.
- No idempotency key exists — avoid duplicate submits; the trash/restore pair is your safe reversal path.
