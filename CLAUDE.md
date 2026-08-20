# BG ARENA — CLAUDE CODEBASE INSTRUCTIONS

# 1. AUTHORITY — P0
This repository is a specification-first engineering project. Markdown documents are implementation contracts. Read the governing documents before changing code. Do not invent product behavior when a documented rule exists.

## 1.1 Priority — P0
- `#` = document authority/domain.
- `##` = major implementation area.
- `###` = required subsystem/rule.
- P0 = invariant; never violate for convenience.
- P1 = required production behavior.
- P2 = preferred implementation quality.
- P3 = future/optional scope.

# 2. REQUIRED READING ORDER — P0
1. `CLAUDE.md`.
2. `docs/00-AI-DEVELOPMENT-MASTER-MANUAL.md`.
3. `docs/01-master-platform-specification.md`.
4. `docs/24-technical-architecture-and-development-blueprint.md`.
5. The domain document matching the task.
6. Relevant database migrations, source files and tests.

# 3. SOURCE OF TRUTH — P0
Explicit approved architecture decisions outrank implementation convenience. The master platform specification defines WHAT BG Arena is. The technical blueprint defines HOW it is built. Domain documents define implementation details. Existing code is authoritative only where it does not conflict with those contracts.

If a conflict affects money, identity, authorization, RLS, payout security, or historical records: STOP, document the conflict, and request an architectural decision. Never silently redefine the system.

# 4. NON-NEGOTIABLE ARCHITECTURE — P0
- Game-agnostic core.
- Supabase PostgreSQL/Auth/RLS.
- Internal wallet currency USD only.
- External deposit currencies are normalized into USD after verified payment.
- Authentication through Google and/or email identity; phone authentication is not used.
- Phone number is profile/contact/payment context only.
- NOWPayments is the crypto deposit adapter.
- CamPay is the mobile-money deposit adapter for MTN Cameroon and Orange Cameroon.
- Mobile Money may be displayed globally, but its current network capability must be clearly described as Cameroon-only.
- Payment providers are adapters; the ledger never depends directly on a provider.
- Immutable/append-only financial history with compensating corrections.
- BG Arena never executes player payouts.
- Payout credentials belong only to the private payout application.
- AI result extraction is untrusted input and never financial authority.

# 5. FORBIDDEN — P0
Never trust client balances, client USD credit values, client exchange rates, client payment status, unauthenticated webhooks, or AI settlement output. Never expose service-role/provider secrets. Never use floating-point arithmetic as authoritative money. Never duplicate-credit a provider event. Never settle a competition twice. Never silently rewrite historical financial facts. Never add permanent PUBG/CODM/Free Fire fields to profiles. Never call payout APIs from BG Arena.

# 6. MONEY RULES — P0
All internal financial values are USD. Use integer minor units or exact decimal arithmetic. Every completed deposit stores original amount, original currency/asset, USD amount, rate, rate source, rate timestamp, provider, provider transaction reference and processing timestamps. Historical credits do not fluctuate with later rates.

# 7. PAYMENT RULES — P0
NOWPayments handles crypto deposits. CamPay handles MTN Cameroon and Orange Cameroon mobile-money deposits. Provider webhooks are server-side, authenticated and idempotent. A payment that cannot be matched confidently becomes `REVIEW_REQUIRED`. A timeout must not create a second provider payment until the original request is reconciled. Underpayments/overpayments follow explicit policy; no silent discrepancy is allowed.

# 8. FINANCIAL MUTATION RULE — P0
Every money mutation must have authorization, validation, idempotency/reference key, transaction boundary, ledger effect, audit event and deterministic failure behavior. UI code may request a financial operation but may never directly write ledger entries or balances.

# 9. RLS — P0
Players can read only their own protected records. Financial writes are server-side. Admin capabilities are role/permission based and audited. Service-role access is never exposed to browser code.

# 10. AI AGENT WORKFLOW — P0
Before coding: identify governing docs, inspect current implementation, map dependencies, state transitions, authorization and failure cases. Then implement the smallest coherent change, write/update tests, run validation, inspect the diff, update docs if behavior changed, and create a Git checkpoint.

# 11. SCOPE CONTROL — P0
A request such as “change the deposit button” authorizes only the smallest UI change required. Do not opportunistically modify ledger, payment, exchange-rate, RLS, schema or architecture code unless the task explicitly requires it.

# 12. DEFINITION OF DONE — P0
A feature is complete only when specification, UI, database schema, business logic, authorization, RLS, validation, persistence, idempotency, auditability, failure states, tests and documentation agree.
