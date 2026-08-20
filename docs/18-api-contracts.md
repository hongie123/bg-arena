# BG ARENA — API CONTRACTS

# 1. API PRINCIPLE — P0
APIs expose use cases, not raw database mutation. Validate input, authenticate, authorize, execute domain service, return typed result/error.

# 2. ROUTE GROUPS — P1
`/api/deposits`; `/api/deposits/[id]`; payment provider create/status routes; provider webhooks; `/api/exchange-rates`; competition/registration endpoints; settlement import/preview/execute; withdrawal request/export/status; notifications/support.

# 3. REQUEST MODEL — P1
Requests use explicit typed schemas. IDs and references have defined formats. Money requests identify currency explicitly even when the internal operation is USD.

# 4. RESPONSE MODEL — P1
Return stable success/error envelopes. Do not leak stack traces, provider secrets or raw sensitive payloads. Pending/processing states are explicit rather than represented as success.

# 5. AUTHORIZATION — P0
Every route determines required actor and permission. Client-supplied user IDs do not override authenticated identity. Admin routes require server-side role/permission checks.

# 6. IDEMPOTENCY — P0
Financial POST operations accept/use stable idempotency keys. Repeating a request with the same key returns the existing safe outcome where possible.

# 7. WEBHOOK ROUTES — P0
Webhooks use provider-specific authentication, schema validation, replay protection and idempotent processing. They do not depend on a browser session.

# 8. STATUS CODES — P1
Use conventional distinctions: 400 validation, 401 unauthenticated, 403 unauthorized, 404 not found/hidden, 409 conflict/duplicate/state conflict, 422 business validation, 429 rate limit, 5xx unexpected/provider infrastructure.

# 9. API VERSIONING — P2
If public contracts become externally consumed, introduce versioning without silently breaking existing clients.

# 10. TESTS — P0
Test authentication, authorization, schema validation, duplicate requests, invalid transitions, provider callbacks and safe error responses.
