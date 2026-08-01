---
name: Analyze a raw message and hunt retroactively
description: Score/analyze a raw email against detection rules, then run a retro hunt across historical message groups.
api: openapi/sublime-security-platform-openapi.json
operations: [analyzeRawMessageLiveFlow, attackScoreForRawMessage, huntMessageGroups, getHuntResults, startHuntJob, getHuntJob, getHuntJobResults]
---

# Analyze a raw message and hunt retroactively

## Auth
`Authorization: Bearer <API key>`. Base URL = your deployment host.

## Analyze a raw message
1. **Live-flow analyze** — `analyzeRawMessageLiveFlow` (`POST /v0/live-flow/raw-messages/analyze`) to run a raw `.eml`/MDM through the rules engine.
2. **Attack score** — `attackScoreForRawMessage` (`POST /v0/messages/attack_score`) for the AI-assisted attack score of a raw message.

## Hunt retroactively
3. **Hunt message groups** — `huntMessageGroups` (`POST /v0/message-groups/hunt`) with an MQL query, then fetch `getHuntResults` (`GET /v0/message-groups/hunt/{id}`).
4. **Hunt jobs (long-running)** — `startHuntJob` (`POST /v0/hunt-jobs`), poll `getHuntJob` (`GET /v0/hunt-jobs/{id}`), then `getHuntJobResults` (`GET /v0/hunt-jobs/{id}/results`).

## Notes
- Hunts are asynchronous — poll the job/hunt id until complete before reading results.
- Pair a hunt with the triage-and-trash skill to remediate matches.
