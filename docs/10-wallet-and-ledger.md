# BG ARENA — WALLET AND IMMUTABLE LEDGER

# 1. FINANCIAL AUTHORITY — P0
The ledger is the financial source of truth. A cached wallet balance is not the authority. Provider dashboards and deposit rows are external evidence, not internal accounting.

# 2. USD ONLY — P0
Every wallet has currency USD. Deposits in XAF, crypto or other supported currencies are converted before internal credit. Entry fees, winnings, refunds and reservations are USD.

# 3. CORE TABLES — P0
wallet_accounts; ledger_entries; financial_transactions; financial_reservations. Keep provider/deposit data separate.

# 4. LEDGER MODEL — P0
A transaction represents a business event. Ledger entries represent its financial effect. Each financial transaction must have a unique reference and a deterministic link to its source object.

# 5. BALANCE — P0
Conceptually: total credits − total debits − active reservations = available balance. Implement this calculation centrally. Do not duplicate formulas in UI pages.

# 6. ATOMIC MUTATION — P0
Financial operation: validate → lock/verify relevant wallet state → create transaction → create ledger entries → create/release reservation where applicable → commit. Failure rolls back all financial changes.

# 7. IDEMPOTENCY — P0
Every externally retried or user-retried financial operation has a stable idempotency/reference key. Duplicate requests return existing outcome or fail safely; they never create a second credit/debit.

# 8. IMMUTABILITY — P0
Never update/delete historical ledger facts to correct mistakes. Use compensating reversal/correction transactions linked to the original.

# 9. RESERVATIONS — P0
Reservations reduce available balance without becoming permanent debit until the operation completes. States: ACTIVE, RELEASED, CONSUMED, CANCELLED.

# 10. MONEY PRECISION — P0
Use integer cents/minor units or exact database numeric arithmetic. Explicitly define rounding mode for conversions and fees. Never use JavaScript `number` as the authoritative accounting representation without a safe conversion strategy.

# 11. TRANSACTION TYPES — P1
DEPOSIT, ENTRY_FEE, SETTLEMENT, REFUND, WITHDRAWAL, ADJUSTMENT, FEE, REVERSAL/CORRECTION. Types must be controlled enums/configuration.

# 12. TESTS — P0
Test zero/negative values, concurrent debits, insufficient funds, duplicate idempotency keys, rollback, reservation release/consume, ledger reconstruction and historical immutability.
