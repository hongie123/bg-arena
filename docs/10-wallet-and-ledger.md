# WALLET AND LEDGER

# 1. ACCOUNTING PRINCIPLE — P0

The wallet is an accounting projection backed by an immutable ledger. Internal currency is USD only. Client-visible balances are never authoritative input.

# 2. BALANCES — P0

Maintain available and reserved amounts. Reserved funds cannot be spent again. The represented balance must reconcile to ledger state.

# 3. LEDGER ENTRIES — P0

Each entry contains unique ID, wallet/user, type, direction, exact amount, currency, source reference, idempotency key, timestamp and metadata. Corrections use compensating transactions.

# 4. ENTRY FEES — P0

Entry fees are USD debits tied to a valid registration and competition. No arbitrary wallet debit is permitted.

# 5. WINNINGS — P0

Winnings are USD credits produced only by authorized settlement. Settlement results and ledger transactions reference each other.

# 6. WITHDRAWAL RESERVATION — P0

Accepted withdrawals move funds from available to reserved through controlled transactional logic, preventing double spending.

# 7. REVERSALS — P0

Failed/cancelled withdrawals release reservation according to the state machine. Completed payouts consume the reserved amount. Every transition is recorded.

# 8. ADMIN ADJUSTMENTS — P0

Admins never edit balances directly. Adjustments are controlled transactions with reason, actor and audit event.

# 9. IDEMPOTENCY — P0

Retrying a financial operation must return the existing outcome instead of changing balance again.

# 10. PRECISION — P0

Use integer minor units or exact decimal arithmetic. Never use binary floating point for authoritative money calculations.

# 11. AI IMPLEMENTATION DIRECTIVES

## P0

All wallet mutations must be trusted server/database operations with transaction boundaries and concurrency protection.

## P1

Unit/integration tests must cover double spending, duplicate events, concurrent withdrawals, reservations and compensating entries.
