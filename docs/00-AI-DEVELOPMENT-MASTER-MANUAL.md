# BG ARENA — AI DEVELOPMENT MASTER MANUAL

# 1. PURPOSE — P0

This is the operating manual for Claude, Antigravity and any other coding agent. The agent must treat the repository as a specification-driven engineering project, not as an open-ended design exercise.

## 1.1 Priority semantics — P0
`#` establishes authority; `##` establishes implementation areas; `###` establishes required rules. P0 rules cannot be weakened. P1 rules are required behavior. P2 rules guide implementation quality. P3 rules are future scope.

# 2. MANDATORY AGENT LOOP — P0

Read → map requirements → inspect code → identify dependencies → design smallest safe change → implement → test → review diff → update docs → checkpoint in Git.

## 2.1 No blind implementation — P0
The agent MUST NOT begin a substantial feature from a short user sentence alone when a governing specification exists. It must read the relevant domain contracts first.

# 3. ARCHITECTURAL BOUNDARIES — P0

UI ≠ financial logic. Provider records ≠ ledger records. External currency ≠ wallet currency. Settlement ≠ withdrawal. BG Arena ≠ payout execution. AI extraction ≠ financial authority. Feature code ≠ global design-system ownership.

# 4. FINANCIAL DEVELOPMENT — P0

Design schema and state machine first; implement provider integration second; verify external events third; convert to USD fourth; create financial transaction and ledger entry atomically fifth; expose UI last. Every mutation must be idempotent and auditable.

# 5. PAYMENT DEVELOPMENT — P0

NOWPayments and CamPay are adapters behind a provider abstraction. Never leak provider-specific credentials into client code. Webhooks are authenticated server events. Provider retries are expected. Unknown payment states remain unknown until verified.

# 6. DATABASE DEVELOPMENT — P0

Migrations are versioned and reviewed. RLS is part of the security boundary. Historical financial records are append-only or corrected through explicit compensating transactions rather than silent edits.

# 7. TEST-FIRST RISK AREAS — P0

Prioritize tests for authentication boundaries, RLS, wallet balance, ledger atomicity, duplicate provider events, deposit idempotency, exchange-rate failure, settlement duplication, withdrawal reservations and ambiguous payout outcomes.

# 8. DOCUMENTATION MAINTENANCE — P1

When implementation changes an architectural contract, update the governing document before or in the same Git checkpoint. Never allow code and specifications to drift silently.
