# CURRENCY AND EXCHANGE RATES

# 1. INTERNAL RULE — P0

USD is the only internal wallet/accounting currency.

# 2. DEPOSIT CONVERSION — P0

At credit time: identify source asset -> obtain configured USD rate -> record rate/source -> calculate exact USD -> apply explicit rounding -> credit wallet -> permanently store conversion snapshot.

# 3. RATE SOURCE — P1

The production rate provider is configuration. Never invent a provider URL or rate.

# 4. CRYPTO PRICING — P1

Preserve asset/network/rate/timestamp and source used for the credit decision.

# 5. RATE FAILURES — P0

If the service is unavailable, invalid or outside configured sanity limits, do not automatically credit. Keep the deposit pending and surface an operational error.

# 6. HISTORICAL AUDIT — P0

Historical deposits are never recalculated later because today's rate differs. The rate used at credit time is immutable financial evidence.

# 7. DISPLAY — P1

Pre-payment USD values are estimates. Only the completed server-side conversion is authoritative.

# 8. AI IMPLEMENTATION DIRECTIVES

## P0

Never trust a client-supplied rate. Never silently choose a fallback rate unless an explicit configured fallback policy exists.

## P1

Use exact decimal arithmetic and persist rate snapshots for audit/reconciliation.
