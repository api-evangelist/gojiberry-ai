---
name: Manage campaigns and lead-source agents
description: List and update campaigns, tune lead-source agents, and read agent run logs using the Gojiberry AI External API.
api: openapi/gojiberry-ai-external-openapi-original.json
operations:
  - CampaignExternalController_findAll
  - CampaignExternalController_update
  - AgentExternalController_findAll
  - AgentExternalController_update
  - AgentExternalController_findLogs
---

# Manage campaigns and lead-source agents

Base URL: `https://ext.gojiberry.ai`. Auth: `Authorization: Bearer <API_KEY>`.
Rate limit: 100 requests/minute per key.

## Steps

1. **List campaigns.** Call `GET /v1/campaign` (`CampaignExternalController_findAll`)
   to review active outreach campaigns.
2. **Adjust a campaign.** Call `PATCH /v1/campaign/{id}`
   (`CampaignExternalController_update`) to change campaign settings.
3. **List lead-source agents.** Call `GET /v1/agent` (`AgentExternalController_findAll`)
   to see the agents that discover and score prospects.
4. **Tune an agent.** Call `PATCH /v1/agent/{id}` (`AgentExternalController_update`)
   to update targeting/variables for a source agent.
5. **Audit agent runs.** Call `GET /v1/agent/{id}/logs`
   (`AgentExternalController_findLogs`) to inspect what an agent did.

## Rules

- `PATCH` operations are write actions — read the current object first and send only
  the fields you intend to change.
- Errors return `{ "message": ..., "statusCode": ... }`; handle `400`/`422` as
  validation failures and `404` as a missing/invisible resource.
- Org owners can operate on a member's campaigns/agents with `x-impersonate-user-id`.
