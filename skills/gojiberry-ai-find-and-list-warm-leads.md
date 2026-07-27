---
name: Find warm leads and build a list
description: Query intent-scored contacts, review intent-type breakdown, create a list, and add the best prospects to it using the Gojiberry AI External API.
api: openapi/gojiberry-ai-external-openapi-original.json
operations:
  - ContactExternalController_findMany
  - ContactExternalController_getIntentTypeCounts
  - ListExternalController_create
  - ContactExternalController_addManyToList
---

# Find warm leads and build a list

Base URL: `https://ext.gojiberry.ai`. Authenticate every request with
`Authorization: Bearer <API_KEY>` (create a key at app.gojiberry.ai → Settings → API).
Rate limit: 100 requests/minute per key.

## Steps

1. **Understand the intent mix.** Call `GET /v1/contact/intent-type-counts`
   (`ContactExternalController_getIntentTypeCounts`) to see how contacts break
   down by intent signal before you filter.
2. **Query warm leads.** Call `GET /v1/contact` (`ContactExternalController_findMany`)
   with `page` and `limit` for pagination and filters such as `search`, `agent`,
   and `dateFrom` to narrow to the highest-intent prospects.
3. **Create a list.** Call `POST /v1/list` (`ListExternalController_create`) to
   create a destination list; keep the returned list `id`.
4. **Add the prospects.** Call `POST /v1/contact/list/{listId}/contacts`
   (`ContactExternalController_addManyToList`) with the selected contact ids.

## Rules

- Errors use a `{ "message": ..., "statusCode": ... }` envelope (not RFC 9457) —
  see `errors/gojiberry-ai-problem-types.yml`.
- A `401` means a missing/invalid key, or an impersonation attempt that breaks the
  org-owner rule. Org owners may act for a member by adding `x-impersonate-user-id`.
- Page through results; do not assume a single response holds every contact.
