# API AND INTEGRATION CONTRACTS

# 1. GENERAL API RULES — P0

Authenticated operations validate input and authorization, return predictable errors and expose only necessary data.

# 2. DOMAIN ENDPOINTS — P1

Contracts may cover profile/account, competitions, registration, wallet/transactions, deposits, exchange rates, withdrawals, notifications, support, admin operations and settlement.

# 3. PAYMENT PROVIDER ADAPTER — P0

Normalize operations such as create payment, query status and validate webhook/event. Provider-specific payloads remain inside adapters.

# 4. EXCHANGE-RATE ADAPTER — P0

Return source asset, USD rate, source/provider, timestamp and validity metadata.

# 5. PAYOUT BOUNDARY — P0

BG Arena exports normalized payout instructions. It does not call payout providers.

# 6. ERROR CONTRACT — P1

Distinguish validation, authentication, authorization, not-found, conflict/idempotency, provider-pending, provider-failure and internal errors. Never expose stack traces/secrets.

# 7. WEBHOOKS — P0

Handlers must authenticate, be retry-safe/idempotent and persist enough information for reliable processing.

# 8. AI IMPLEMENTATION DIRECTIVES

## P0

Do not leak provider payloads or secrets into unrelated domains. Never invent production endpoints.

## P1

Use typed request/response schemas and explicit error codes.
