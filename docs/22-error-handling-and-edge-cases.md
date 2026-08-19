# ERROR HANDLING AND EDGE CASES

## General

Errors must leave the system in a valid state. Prefer explicit pending states over guessing.

## Payments

Handle duplicate webhooks, delayed confirmations, provider timeouts, expired invoices, underpayment, overpayment, rate-service failure, provider reversal and conflicting status events.

Never credit twice. Never assume timeout means failure.

## Wallet

Prevent negative balances and double spending. Concurrent withdrawals/entries must use transactional locking or equivalent concurrency control.

## Registration

Handle full competition, closing race conditions, duplicate registration and wallet debit failure atomically.

## Settlement

Reject malformed/ambiguous results. Prevent double settlement. If settlement processing crashes, the retry must safely resume or return the existing outcome.

## Withdrawal

Handle insufficient funds, duplicate requests, invalid destination, payout pending, payout failed, payout ambiguous and administrator rejection. Never release funds simply because an external request timed out.

## Account

Suspended accounts should be blocked from restricted actions while preserving historical financial records.

## Admin

Admin actions must fail closed when authorization cannot be verified.

## External services

Use timeouts, controlled retries and idempotency. Never retry a financial mutation blindly unless the operation is designed to be idempotent.

## Data correction

Do not delete historical financial records. Use compensating entries and audit logs.
