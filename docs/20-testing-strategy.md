# BG ARENA — TESTING STRATEGY

# 1. TESTING PRINCIPLE — P0
Financial correctness and authorization are release blockers. A passing visual page is not sufficient.

# 2. TEST LEVELS — P1
Unit tests for pure logic; integration tests for database/domain services; security tests for RLS/authorization; payment tests for adapters/webhooks; financial tests for ledger invariants; E2E tests for critical user journeys.

# 3. FINANCIAL TESTS — P0
Verify deposit credits exactly once; entry fee debit is atomic; settlement credits exactly once; withdrawal reservations cannot exceed available funds; compensating corrections preserve history.

# 4. PAYMENT TESTS — P0
Success webhook, duplicate webhook, fake signature, malformed payload, provider timeout, status polling recovery, wrong currency, amount mismatch, unmatched deposit, provider outage and retry.

# 5. EXCHANGE TESTS — P0
Rate unavailable, invalid rate, rounding, fallback provider, unsupported currency, stale estimate versus authoritative rate and snapshot immutability.

# 6. SECURITY TESTS — P0
Cross-user access, forged role, client balance manipulation, direct ledger writes, webhook bypass, secret exposure and IDOR.

# 7. COMPETITION TESTS — P1
Registration window, capacity race, duplicate registration, insufficient balance, cancellation, dynamic-field validation and settlement lifecycle.

# 8. WITHDRAWAL TESTS — P0
Duplicate request, concurrent request, reservation lifecycle, admin authorization, success/failure/unknown payout outcome and safe recovery.

# 9. REGRESSION RULE — P0
Any change touching financial/security code must run the relevant existing suite plus new regression tests. Do not delete failing tests merely to make CI green.

# 10. DEFINITION OF DONE — P0
All relevant tests pass; typecheck/lint pass; migration tests pass; security assumptions are verified; no known P0/P1 failure remains.
