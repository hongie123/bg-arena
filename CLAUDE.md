# BG ARENA — CLAUDE CODEBASE INSTRUCTIONS

# 1. DOCUMENT AUTHORITY — P0

This repository is an executable engineering contract for BG Arena. Markdown documentation is not a summary. It defines architecture, business rules, security boundaries, data ownership, financial behavior, UI responsibilities, testing obligations, and AI-agent modification limits.

## 1.1 Priority system — P0
- `#` = authoritative document/domain.
- `##` = major implementation area.
- `###` = required subsystem/rule.
- `P0` = non-negotiable invariant. Never weaken it for convenience.
- `P1` = required product behavior.
- `P2` = preferred engineering behavior.
- `P3` = future/optional behavior.

# 2. REQUIRED READING ORDER — P0

Before changing code, read `CLAUDE.md`, `docs/00-AI-DEVELOPMENT-MASTER-MANUAL.md`, `docs/01-master-platform-specification.md`, `docs/24-technical-architecture-and-development-blueprint.md`, then every domain document affected by the task. Inspect migrations, source, tests and configuration before editing.

# 3. SOURCE OF TRUTH — P0

Final explicit user decisions outrank implementation convenience. Then this file, the AI manual, the master platform specification, the technical blueprint, specialized specifications, existing migrations/code, and finally engineering judgment. If a conflict affects money, identity, authorization, RLS, settlement, withdrawal or provider security, STOP and document the conflict. Never silently redefine architecture.

# 4. CORE INVARIANTS — P0

## 4.1 Game-agnostic platform — P0
Core services MUST NOT contain permanent PUBG/CODM/Free Fire/game-specific profile or scoring assumptions. Game-specific information belongs to competition registration fields and external result/settlement processing.

## 4.2 Stack — P0
Frontend: Next.js + React + TypeScript. Backend: Next.js server-side services + Supabase. Database: Supabase PostgreSQL. Authentication: Supabase Auth with Google and email identity. Version control: Git/GitHub. Development environment: Antigravity + Claude.

## 4.3 USD-only accounting — P0
Every authoritative wallet is USD. Incoming BTC, USDT, ETH, XAF, MTN Cameroon or Orange Cameroon payments remain external payment records. A completed deposit becomes a fixed USD ledger credit with the original amount, currency/asset, rate, source, timestamps and provider references preserved.

## 4.4 Payments — P0
NOWPayments handles crypto deposits. CamPay handles MTN Cameroon and Orange Cameroon Mobile Money. Mobile Money may be visible globally only with an explicit Cameroon-only limitation. Do not claim unsupported networks/countries.

## 4.5 Authentication — P0
Phone authentication is disabled. Phone numbers are profile/contact/payment information only.

## 4.6 Payout separation — P0
BG Arena never executes payout-provider APIs. It validates withdrawals, reserves USD, records state, generates payout instructions and reconciles results. The private payout application owns payout-provider secrets and execution.

# 5. FINANCIAL RULES — P0

Every money mutation requires server authorization, validation, unique reference/idempotency, transaction boundary, explicit ledger impact, duplicate protection and auditability. Use exact decimal or integer minor-unit arithmetic. The immutable ledger is the financial source of truth. Never trust client balances, client USD amounts, client status, client exchange rates or browser redirects.

# 6. PAYMENT RULES — P0

Validate webhook authenticity, provider transaction/reference, deposit association, expected amount/currency/asset, status, duplicate state and conversion requirements before credit. Webhooks are server-side only. A timeout MUST NOT blindly create a second provider payment. Unmatched, ambiguous, underpaid, overpaid or wrong-currency payments enter controlled review according to the configured policy.

## 6.1 Deposit chain — P0
Player → deposit intent → provider payment → provider confirmation → verified provider transaction → exchange-rate snapshot → USD financial transaction → immutable ledger credit → wallet → notification.

## 6.2 Historical conversion — P0
Never recalculate a completed deposit using a later rate. Rate failures produce `CONVERSION_PENDING` or another explicitly defined safe state; never guess.

# 7. SETTLEMENT RULES — P0

AI extraction is untrusted. Validate schema, competition, registrations, participants, duplicates, ranges, eligibility, game-specific rules and financial totals. Require administrator preview and explicit confirmation. Execute the final ledger mutation atomically and prevent duplicate settlement.

# 8. WITHDRAWAL RULES — P0

Validate available balance, create a reservation before payout export, preserve the reservation during ambiguous external outcomes, and reconcile through an authorized workflow. Do not place payout secrets in BG Arena.

# 9. RLS AND AUTHORIZATION — P0

Players can read their own profile, wallet, deposits, transactions, registrations, withdrawals, notifications, support and disputes. They cannot insert ledger entries, complete deposits, execute settlements, access other users' financial records or administer payment methods. Server authorization and PostgreSQL/RLS protections must back the UI.

# 10. AI AGENT WORKFLOW — P1

1. Read governing docs.
2. Inspect current implementation.
3. Identify ownership and dependencies.
4. Define schema/state transitions/security.
5. Implement server/domain behavior first for financial features.
6. Implement UI within the design system.
7. Add unit/integration/security/E2E tests.
8. Test retries, duplicates, failures and authorization.
9. Review the diff.
10. Update documentation when the contract changes.

# 11. FORBIDDEN — P0

Never expose secrets; use floating-point authoritative money; trust browser financial values; bypass RLS; duplicate-credit a provider event; silently modify historical ledger records; hardcode provider assumptions; execute payouts; let AI directly credit wallets; add permanent game-specific profile fields; or invent missing business rules.

# 12. DEFINITION OF DONE — P0

A feature is complete only when documentation, schema, domain logic, API behavior, UI, validation, authorization/RLS, persistence, idempotency, auditability, error handling and tests agree. A visually complete screen, mocked provider success or client-side balance is not a completed feature.
