---
name: Author and activate an MQL detection rule
description: Validate, create, and activate a Sublime MQL detection rule, then confirm it in the rule list.
api: openapi/sublime-security-platform-openapi.json
operations: [validateRule, createRule, activateRule, listRules, getRule, deactivateRule]
---

# Author and activate an MQL detection rule

Sublime is a programmable rules engine; detection rules are written in Message Query Language (MQL).

## Auth
`Authorization: Bearer <API key>`. Base URL = your deployment host.

## Steps
1. **Validate first** — `validateRule` (`POST /v0/rules/validate`) to check MQL syntax before persisting.
2. **Create** — `createRule` (`POST /v0/rules`) with the rule source, name, and severity.
3. **Activate** — `activateRule` (`POST /v0/rules/{id}/activate`) so it evaluates live traffic.
4. **Confirm** — `listRules` (`GET /v0/rules`) or `getRule` (`GET /v0/rules/{id}`) to verify state.
5. **Roll back** — `deactivateRule` (`POST /v0/rules/{id}/deactivate`) to stop evaluation without deleting.

## Notes
- Always `validateRule` before `createRule` to surface MQL errors early.
- Rule history is available via `getRuleHistory` (`GET /v0/rules/{id}/rule-history`).
