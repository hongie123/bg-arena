# PAYMENTS AND DEPOSITS

# 1. LAUNCH METHODS — P1

Cameroon launch: MTN Mobile Money, Orange Money and cryptocurrency through the configured provider. Integrations are adapter-based.

# 2. PAYMENT STATES — P0

Use explicit states such as CREATED, PENDING, CONFIRMED, PROCESSING, CREDITED, FAILED, EXPIRED, CANCELLED and REVERSED as required by the provider flow.

# 3. PROOF OF PAYMENT — P0

Browser redirects/client callbacks are not authoritative. Credit only from trusted provider confirmation/webhook or controlled authorized administrative confirmation.

# 4. PROVIDER EVENTS — P0

Verify provider authentication/signatures, capture provider payment/event IDs, enforce idempotency, persist event state and perform one atomic USD wallet credit.

# 5. CAMEROON MOBILE MONEY — P1

MTN Mobile Money and Orange Money are Cameroon-specific. Country/payment-method availability must be configuration-driven so international methods can be added later.

# 6. CRYPTO — P1

Preserve asset, network, expected/actual amount, provider invoice/payment ID, transaction hash when available, confirmation state and final USD conversion. Never credit merely because an address was displayed.

# 7. FEES — P1

Provider/platform/network fees must be explicit. Never silently alter credited amounts.

# 8. RECONCILIATION — P0

Every deposit must retain original amount/asset, conversion rate/source/time, provider references and resulting ledger transaction. Provider records must be reconcilable to internal deposit IDs.

# 9. AI IMPLEMENTATION DIRECTIVES

## P0

Do not invent provider API behavior. Provider-specific payloads stay inside adapters, and secrets stay server-side.

## P1

Implement provider interfaces so adding another payment method does not modify wallet accounting logic.
