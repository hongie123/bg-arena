# TESTING AND QUALITY ASSURANCE

# 1. UNIT TESTS — P0

Cover money arithmetic, conversion, state transitions, eligibility, registration validation, settlement calculations and idempotency.

# 2. INTEGRATION TESTS — P0

Test Supabase interactions, RLS, authentication, wallet transactions, registration/payment relationships and admin authorization.

# 3. PAYMENT TESTS — P0

Simulate success, pending, failure, duplicate webhook, out-of-order webhook, timeout and reconciliation scenarios.

# 4. WITHDRAWAL TESTS — P0

Test insufficient balance, duplicates, reservation, payout success/failure, ambiguous outcomes, release and retry.

# 5. SETTLEMENT TESTS — P0

Test malformed JSON, unknown players, duplicate participants, invalid metrics, correct calculations, duplicate settlement and correction flows.

# 6. SECURITY TESTS — P0

Verify players cannot access other users' data, alter balances, elevate roles, confirm settlements or approve withdrawals.

# 7. UI TESTS — P1

Test responsive navigation, loading/empty/error states, validation, confirmation dialogs and financial formatting.

# 8. DEFINITION OF DONE — P0

A feature is complete only when happy path, failure paths, authorization, persistence, idempotency and appropriate UI states are tested.

# 9. AI IMPLEMENTATION DIRECTIVES

## P0

Financial and authorization tests are mandatory before declaring production readiness.

## P1

Prefer deterministic tests with isolated fixtures and explicit duplicate/concurrency cases.
