# BG ARENA — SYSTEM ARCHITECTURE

# 1. ARCHITECTURAL LAYERS — P0

BG Arena is separated into presentation/client, authenticated API/application, domain/business services, persistence/database, external provider adapters, asynchronous/event processing and administration tooling. The client is never a trusted business layer.

# 2. DOMAIN BOUNDARIES — P0

Primary domains: identity/profiles, competitions, registrations, wallet/ledger, payments/deposits, exchange rates, withdrawals, settlement, notifications, support, administration and audit.

Each domain exposes explicit operations rather than unrelated UI components directly mutating financial records.

# 3. REQUEST FLOW — P0

Authenticated request:

`client -> authentication -> server authorization -> validation -> domain operation -> database -> response`

Payment event:

`provider -> verified event endpoint -> validation -> idempotency -> payment state -> rate processing -> ledger credit -> notification`

# 4. TRANSACTION BOUNDARIES — P0

Money-changing operations must be atomic. Wallet credit, entry-fee debit, withdrawal reservation and settlement must never leave partially completed financial state.

Use PostgreSQL transactions/functions or secure Supabase server-side mechanisms appropriate to the operation.

# 5. EVENT/IDEMPOTENCY ARCHITECTURE — P0

Every provider event capable of financial mutation requires a durable unique event/provider reference. Retries must return the existing outcome without creating another financial mutation.

# 6. PUBLIC/PRIVATE SERVICE SEPARATION — P0

BG Arena is the public application. The private payout application owns payout-provider secrets and external payout execution. BG Arena produces normalized payout instructions and records outcomes.

# 7. GAME ADAPTER ARCHITECTURE — P0

Game-specific registration fields, result fields, validation and settlement rules belong to configurable game/competition adapters. Core wallet/user services consume normalized outputs.

# 8. DATABASE ACCESS — P0

RLS controls user-facing access. Privileged operations are server-side. The Supabase service-role key is never shipped to the browser.

# 9. OBSERVABILITY — P1

Log payment events, ledger operations, settlement attempts, withdrawal transitions, authorization failures, provider failures and webhook verification failures without secrets or unnecessary sensitive data.

# 10. FAILURE PHILOSOPHY — P0

External dependencies require explicit pending, failed, retryable and ambiguous states. A timeout must not be interpreted as a financial failure unless provider reconciliation establishes that result.

# 11. AI IMPLEMENTATION DIRECTIVES

## P0

Preserve domain boundaries. Never move financial authority into React/components/browser code.

## P1

Implement provider adapters, service boundaries and transaction operations before building complex UI workflows.

## P2

Prefer typed domain contracts and small testable services. Keep external payload translation inside adapters.
