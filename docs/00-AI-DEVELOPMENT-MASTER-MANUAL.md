# BG ARENA — AI DEVELOPMENT MASTER MANUAL

# 1. PURPOSE — P0
This manual defines how Claude, Antigravity and other coding agents must operate inside BG Arena. The agent is an implementer, not the architectural authority. Documentation is a contract.

# 2. PRIORITY SYSTEM — P0
## 2.1 Heading semantics — P0
`#` identifies authoritative domains; `##` identifies implementation areas; `###` identifies required rules or procedures.
## 2.2 Priority semantics — P0
P0 is mandatory and security/financially critical. P1 is required product behavior. P2 is preferred implementation quality. P3 is optional/future. A lower priority never overrides a higher priority.

# 3. REQUIRED READING — P0
Before a substantial task, read CLAUDE.md, this manual, the master specification, the technical blueprint, the relevant domain document, affected migrations/source and tests. For financial work also read wallet, payments, exchange rates, reconciliation and security documents.

# 4. MANDATORY AGENT LOOP — P0
### Step 1: classify task
Identify domain, risk level, files likely affected, and whether it touches money, identity, RLS, provider credentials or architecture.
### Step 2: build requirement map
List required behavior, forbidden behavior, data inputs/outputs, state transitions, permissions, failure states and tests.
### Step 3: inspect repository
Read existing implementation and migrations before creating replacements. Preserve working behavior not contradicted by the specification.
### Step 4: design
Define data model, service boundaries, transaction boundaries, authorization, idempotency and UI states before coding.
### Step 5: implement
Keep provider-specific and financial logic behind domain/infrastructure boundaries. Keep UI free of authoritative financial mutations.
### Step 6: verify
Run type checking, linting, unit tests, integration tests and security/financial tests relevant to the change.
### Step 7: review
Inspect Git diff for accidental scope expansion, secret leakage, client/server boundary mistakes and undocumented behavior.
### Step 8: document/checkpoint
Update contracts when behavior changes and create a coherent Git checkpoint.

# 5. CHANGE AUTHORITY — P0
The agent may implement documented behavior. It may not silently change architecture. If a requirement is ambiguous, especially around financial policy, provider behavior or settlement, choose the safe state and flag the ambiguity rather than guessing.

# 6. FINANCIAL WORKFLOW — P0
Schema/state machine → provider event → verification → idempotency → conversion → financial transaction → ledger → notification/reconciliation. Never reverse this order by starting with UI and inventing finance later.

# 7. FAILURE-FIRST DEVELOPMENT — P0
For each external operation define timeout, retry, duplicate event, malformed input, unavailable provider, mismatched amount, wrong currency, unknown state and partial-failure behavior before implementation.

# 8. SECURITY REVIEW — P0
For every feature ask: who can invoke it, what can they read, what can they mutate, what happens if the client is malicious, what happens if the request is replayed, and what secrets exist? Encode these answers in RLS, server checks, constraints and tests.

# 9. UI DEVELOPMENT — P1
Every asynchronous screen must define loading, empty, success, partial and error states. Every financial screen must identify USD clearly and distinguish estimates from confirmed values. Do not display provider success as wallet success until the server confirms the ledger state.

# 10. TEST PRIORITY — P0
Highest risk tests: authentication boundaries, RLS, ledger atomicity, duplicate deposits, webhook authentication, exchange-rate failure, settlement duplication, withdrawal reservations, payout ambiguity and administrative overrides.

# 11. DEFINITION OF DONE — P0
No feature is complete because a page renders. Completion requires the documented domain behavior, persistence, security, authorization, validation, error handling, tests, auditability and documentation to agree.
