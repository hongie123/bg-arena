# BG ARENA — CLAUDE CODEBASE INSTRUCTIONS

# 1. PURPOSE — P0

This repository is the authoritative implementation specification for BG Arena. Any AI coding agent working here MUST treat the Markdown documentation as an engineering contract, not as general product notes.

# 2. REQUIRED READING ORDER — P0

Before creating or modifying application code, read:

1. `CLAUDE.md` — global engineering rules.
2. `docs/00-AI-DEVELOPMENT-MASTER-MANUAL.md` — AI operating manual and priority system.
3. `docs/01-master-platform-specification.md` — product source of truth.
4. The specialized domain document relevant to the requested feature.
5. Existing code, migrations and tests affected by the change.

# 3. PRIORITY MARKERS — P0

Markdown heading levels communicate scope and priority:

- `#` = document/domain authority.
- `##` = major implementation area.
- `###` = required subsystem/rule.
- `P0` = non-negotiable invariant/security/financial requirement.
- `P1` = required product behavior.
- `P2` = preferred engineering implementation.
- `P3` = optional/future behavior.

If a P0 rule conflicts with convenience, convenience loses.

# 4. SOURCE-OF-TRUTH HIERARCHY — P0

1. Explicit user decisions.
2. This file.
3. `docs/00-AI-DEVELOPMENT-MASTER-MANUAL.md`.
4. `docs/01-master-platform-specification.md`.
5. Specialized documents.
6. Existing code.
7. Engineering judgment.

When a conflict affects money, identity, authorization, database integrity, or payout security, STOP and report the conflict instead of guessing.

# 5. NON-NEGOTIABLE ARCHITECTURE — P0

- BG Arena is game-agnostic.
- Supabase/PostgreSQL is the planned primary backend.
- Internal accounting is USD-only.
- Deposits may originate in XAF, other configured fiat currencies, or crypto, but the wallet is USD.
- Original deposit amount, currency/asset, conversion rate, rate source, timestamps and provider references are preserved.
- Cameroon launch payment methods are MTN Mobile Money and Orange Money, explicitly identified as Cameroon-only.
- Crypto is provider-adapter based.
- Authentication is email based.
- Phone numbers are contact/profile/payment information only.
- BG Arena does not execute payout-provider calls.
- Payout-provider credentials belong only to the private payout application.
- Financial mutations are ledger-backed and idempotent.
- Admin financial operations require authorization and audit logging.

# 6. FORBIDDEN IMPLEMENTATIONS — P0

Never:

- trust client-supplied wallet balances;
- trust client-supplied USD credit amounts;
- trust client-supplied exchange rates;
- expose Supabase service-role keys in the browser;
- expose payment/webhook/payout secrets;
- use floating-point arithmetic as authoritative money;
- credit the same provider event twice;
- settle the same competition twice;
- silently edit/delete historical financial records;
- add PUBG/CODM/Free Fire-specific permanent profile columns;
- execute payout APIs from BG Arena;
- invent undocumented provider endpoints or credentials.

# 7. IMPLEMENTATION WORKFLOW — P1

For every feature:

1. Identify specification documents.
2. Read dependencies.
3. Inspect current implementation.
4. Define data model and state transitions.
5. Define authorization and RLS requirements.
6. Define validation and failure behavior.
7. Implement backend/domain behavior.
8. Implement UI.
9. Add tests.
10. Review security and financial implications.
11. Update documentation if the contract changed.

# 8. FINANCIAL SAFETY — P0

Every money-changing operation must have an explicit reference/idempotency key, authorization, validation, transaction boundary, ledger impact and audit trail.

Use integer minor units or exact decimal arithmetic. Preserve the rate used at deposit processing time. Never recalculate historical credits using a later rate.

# 9. SETTLEMENT SAFETY — P0

AI-extracted results are untrusted input. Validate JSON/schema, competition identity, registration identity, duplicate participants, numeric ranges, game-specific rules and calculated amounts. AI must never directly credit a wallet.

Settlement requires administrator preview and explicit confirmation before financial mutation.

# 10. WITHDRAWAL SAFETY — P0

Withdrawal funds are reserved before pending payout processing. Ambiguous external payout outcomes must remain reserved until reconciled. BG Arena exports payout instructions; it does not execute the payout provider.

# 11. UI REQUIREMENTS — P1

Player navigation:

- Dashboard
- Games/Tournaments
- Wallet
- Results & History
- Notifications
- Tutorials
- Support
- Account/Settings

Admin navigation:

- Overview
- Users
- Competitions
- Registrations
- Settlements
- Payments
- Wallet/Transactions
- Withdrawals
- Financial Reconciliation
- Notifications
- Support

All asynchronous views need loading, empty, success and error states. Money displays must clearly identify USD.

# 12. DEVELOPMENT ORDER — P1

Build in dependency order:

1. foundation/configuration;
2. Supabase schema/migrations;
3. authentication/RLS;
4. profiles/roles;
5. wallet/ledger;
6. deposits/provider adapters;
7. exchange-rate processing;
8. competitions/registrations;
9. player dashboard;
10. admin dashboard;
11. result extraction/settlement;
12. withdrawals/payout export;
13. reconciliation;
14. notifications/support;
15. tests/security/observability/deployment.

# 13. DEFINITION OF DONE — P0

A feature is complete only when its UI, database schema, business rules, authorization, RLS, validation, error handling, persistence, auditability, idempotency, tests and documentation agree.

A screen that only looks correct is not a completed feature.
