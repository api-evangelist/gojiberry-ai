---
name: Enrich a contact and send a LinkedIn message
description: Look up a contact, enrich its email, then open the Unibox thread and send a personalized LinkedIn message via the Gojiberry AI External API.
api: openapi/gojiberry-ai-external-openapi-original.json
operations:
  - ContactExternalController_findOne
  - ContactExternalController_enrichEmail
  - UniboxExternalController_getThreads
  - UniboxExternalController_sendMessage
---

# Enrich a contact and send a LinkedIn message

Base URL: `https://ext.gojiberry.ai`. Auth: `Authorization: Bearer <API_KEY>`.
Rate limit: 100 requests/minute per key.

## Steps

1. **Load the contact.** Call `GET /v1/contact/{id}`
   (`ContactExternalController_findOne`) to read the prospect's current data.
2. **Enrich the email.** Call `POST /v1/contact/{id}/enrich/email`
   (`ContactExternalController_enrichEmail`) to resolve/verify a work email.
3. **Find the conversation.** Call `GET /v1/unibox/threads`
   (`UniboxExternalController_getThreads`) to locate the thread for this contact.
4. **Send the message.** Call `POST /v1/unibox/messages/send-message`
   (`UniboxExternalController_sendMessage`) with a hyper-personalized LinkedIn
   message grounded in the contact's intent/social signals.

## Rules

- Sending a message has an external consequence (a real LinkedIn message goes out) —
  confirm intent before calling `sendMessage`; treat it as an acting/write operation.
- `404` means the contact/thread is not visible to the authenticated (or impersonated)
  user — verify ids and org membership.
- Respect the 100 req/min limit when enriching or messaging in bulk.
