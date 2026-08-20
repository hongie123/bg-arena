# ERROR HANDLING AND EDGE CASES

# 1. GENERAL — P0

Errors must leave the system valid. Prefer explicit pending states over guessing.

# 2. PAYMENTS — P0

Handle duplicate webhooks, delayed confirmation, provider timeouts, expired invoices, under/overpayment, rate-service failure, reversal and conflicting status events. Never credit twice; timeout does not automatically mean failure.

# 3. WALLET — P0

Prevent negative balances and double spending. Concurrent withdrawals/entries require transactional locking or equivalent concurrency protection.

# 4. REGISTRATION — P0

Handle full competitions, closing race conditions, duplicate registration and wallet debit failure atomically.

# 5. SETTLEMENT — P0

Reject malformed/ambiguous results. Prevent double settlement. Retries must safely resume or return existing outcome.

# 6. WITHDRAWAL — P0

Handle insufficient funds, duplicate requests, invalid destinations, pending/failed/ambiguous payout and admin rejection. Never release funds solely because a provider request timed out.

# 7. ACCOUNT — P1

Suspended accounts are blocked from restricted actions while financial history remains intact.

# 8. ADMIN — P0

Admin actions fail closed when authorization cannot be verified.

# 9. EXTERNAL SERVICES — P0

Use timeouts, controlled retries and idempotency. Never blindly retry a non-idempotent financial mutation.

# 10. DATA CORRECTION — P0

Never delete historical financial records. Use compensating entries and audit logs.

# 11. AI IMPLEMENTATION DIRECTIVES

## P0

Every external dependency must have explicit failure/ambiguous states.

## P1

User-facing errors should be safe, actionable and free of secrets/internal stack traces.
