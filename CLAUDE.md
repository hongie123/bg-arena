# BG ARENA — CLAUDE CODEBASE INSTRUCTIONS

# 1. DOCUMENT STATUS — P0

This repository is the authoritative engineering specification for BG Arena. These Markdown files are the implementation contract for AI coding agents and human developers. They are not a short product summary.

The application MUST be implemented from these specifications. An agent MUST NOT replace a specified behavior with a simpler approximation merely because it is easier.

Priority markers:

- `#` = document/domain authority.
- `##` = major implementation area.
- `###` = required subsystem or rule.
- `P0` = non-negotiable security, financial, identity, integrity or architectural invariant.
- `P1` = required product behavior.
- `P2` = preferred engineering behavior.
- `P3` = optional/future behavior.

If a P0 rule conflicts with convenience, convenience loses.

# 2. REQUIRED READING ORDER — P0

Before creating, modifying, deleting or refactoring application code, the agent MUST read:

1. `CLAUDE.md`.
2. `docs/00-AI-DEVELOPMENT-MASTER-MANUAL.md`.
3. `docs/01-master-platform-specification.md`.
4. The specialized domain document(s) relevant to the task.
5. Existing source code, migrations, configuration and tests affected by the task.

If the task crosses multiple domains, read every affected domain document before implementation.

# 3. SOURCE-OF-TRUTH HIERARCHY — P0

When requirements appear to conflict, use this order:

1. Explicitly recorded final user decisions.
2. `CLAUDE.md`.
3. `docs/00-AI-DEVELOPMENT-MASTER-MANUAL.md`.
4. `docs/01-master-platform-specification.md`.
5. Specialized domain specifications.
6. Existing migrations and code.
7. Engineering judgment.

If a conflict affects money, wallet balances, identity, authorization, RLS, database integrity, settlement, withdrawals or payout security, STOP. Do not silently choose an interpretation.

# 4. CORE PLATFORM INVARIANTS — P0

## 4.1 Game-agnostic architecture — P0

BG Arena is a game-agnostic competitive gaming platform. Core users, wallets, competitions, registrations, payments, withdrawals, notifications, support and administration MUST NOT contain permanent PUBG, CODM, Free Fire or other game-specific profile assumptions.

Game-specific information belongs to competition-specific registration fields, competition configuration and game-specific settlement/result adapters.

## 4.2 Backend — P0

Supabase/PostgreSQL is the planned primary backend. Database rules, constraints, transactions, functions and Row Level Security MUST be treated as part of the security boundary.

## 4.3 Internal currency — P0

BG Arena accounting is USD-only.

Deposits can originate from configured local fiat currencies or crypto assets. The original amount, source currency/asset, exchange rate, rate source, timestamp and provider reference MUST be preserved. The player's authoritative wallet credit is USD.

## 4.4 Cameroon payment methods — P0

The initial Cameroon launch supports:

- MTN Mobile Money Cameroon.
- Orange Money Cameroon.
- Crypto through the configured crypto payment provider/adapter.

MTN Mobile Money and Orange Money are Cameroon-specific payment methods. The UI may show these methods to users outside Cameroon only where product configuration requires it, but MUST clearly state that the mobile-money method is available only for Cameroon and MUST collect/validate the required Cameroon payment information before processing.

Crypto is intended to be globally available where the selected provider supports the user's jurisdiction and asset.

## 4.5 Authentication — P0

Authentication is email-based. Phone numbers are NOT the authentication credential. Phone numbers may be collected as profile/contact/payment information and for mobile-money transactions.

## 4.6 Payout separation — P0

BG Arena MUST NOT directly execute payout-provider APIs. BG Arena validates withdrawals, reserves funds, records the withdrawal state, maintains financial history and generates payout instructions/data for the separate private payout application.

Payout-provider credentials and execution secrets belong exclusively to the private payout application.

# 5. FORBIDDEN IMPLEMENTATIONS — P0

Never:

- trust a client-supplied wallet balance;
- trust a client-supplied USD credit amount;
- trust a client-supplied exchange rate;
- expose a Supabase service-role key in browser/client code;
- expose payment, webhook or payout secrets;
- use JavaScript floating-point arithmetic as authoritative money accounting;
- credit the same provider event more than once;
- process the same withdrawal or settlement twice;
- silently edit or delete historical financial records;
- add permanent game-specific profile columns;
- execute payout-provider APIs from BG Arena;
- invent provider endpoints, credentials or undocumented integration behavior;
- let an AI result directly mutate a wallet;
- bypass RLS with client-side assumptions;
- mark a payment successful merely because the client says it succeeded;
- recalculate historical wallet credits using a later exchange rate.

# 6. MONEY AND LEDGER SAFETY — P0

Every money-changing operation MUST have:

1. A unique operation/reference/idempotency key.
2. Server-side authorization.
3. Server-side validation.
4. A transaction boundary.
5. An explicit ledger impact.
6. An audit trail where applicable.
7. Duplicate protection.
8. Deterministic failure behavior.

Use integer minor units or exact decimal arithmetic. Never use binary floating-point as the authoritative financial representation.

Wallet balance MUST be derivable from authoritative financial records. A cached/display balance MUST never become an independent source of truth.

# 7. PAYMENT SAFETY — P0

A payment provider callback/webhook is untrusted until authenticated and validated.

The system MUST verify:

- provider event authenticity/signature where supported;
- provider transaction/reference ID;
- expected payment intent/deposit record;
- expected amount/currency or asset;
- payment status;
- duplicate-event state;
- user/deposit ownership;
- supported method/provider;
- conversion requirements before wallet credit.

A provider event may be retried. Processing MUST therefore be idempotent.

Do not credit a wallet from a browser redirect alone. Redirects may update UI state, but authoritative payment confirmation comes from validated provider information.

# 8. CURRENCY CONVERSION — P0

The system MUST preserve the exact conversion information used for each deposit:

- source currency or crypto asset;
- source amount;
- USD credited amount;
- exchange rate;
- rate source/provider;
- rate timestamp;
- conversion timestamp;
- provider/payment reference;
- fees where applicable.

Once a deposit is credited, its historical USD credit MUST NOT change because a later exchange rate changes.

# 9. AUTHORIZATION AND RLS — P0

Players may access only their own private records unless an explicitly public record is intended.

Administrative actions MUST require an authenticated administrator role and server/database authorization.

Sensitive operations MUST be enforced server-side and, where appropriate, by PostgreSQL constraints/functions and RLS—not merely hidden UI buttons.

Never assume that hiding an admin control is authorization.

# 10. SETTLEMENT SAFETY — P0

AI-extracted match results are untrusted input.

Before settlement, BG Arena MUST validate:

- JSON/schema structure;
- competition identity;
- registration identity;
- participant existence;
- duplicate participants;
- permitted numeric ranges;
- competition status;
- game-specific rules;
- prize/fee calculations;
- settlement totals;
- duplicate settlement protection;
- eligibility and disqualification rules.

AI MUST NEVER directly credit player wallets.

Settlement MUST follow the sequence:

1. Import result data.
2. Validate structure.
3. Validate participants and competition.
4. Calculate expected financial results.
5. Present administrator preview.
6. Require explicit administrator confirmation.
7. Execute an atomic financial mutation.
8. Record settlement and audit information.
9. Prevent repeat settlement.

# 11. WITHDRAWAL SAFETY — P0

A withdrawal request MUST be validated against available funds before reservation.

Funds MUST be reserved before the withdrawal is exported for payout processing.

If an external payout result is ambiguous, the reserved funds MUST remain reserved until an authorized reconciliation process resolves the state.

BG Arena exports payout instructions. It does not execute payout-provider calls.

# 12. UI REQUIREMENTS — P1

Player navigation MUST support:

- Dashboard
- Games/Tournaments
- Wallet
- Results & History
- Notifications
- Tutorials
- Support
- Account/Settings

Admin navigation MUST support:

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

Every asynchronous screen MUST have intentional loading, empty, success and error states.

All authoritative financial values MUST clearly display USD. Local deposit input may display the original local currency/asset alongside the resulting USD amount.

# 13. UI AND UX CONSISTENCY — P1

The UI MUST be responsive and usable on mobile and desktop.

Forms MUST provide validation before submission and server-error handling after submission.

Destructive or financial actions MUST require explicit confirmation where specified by the relevant domain document.

The UI MUST never imply that a transaction succeeded before authoritative persistence/provider confirmation has occurred.

# 14. IMPLEMENTATION WORKFLOW — P1

For every feature:

1. Identify the governing specification.
2. Read all dependencies.
3. Inspect the current implementation.
4. Define data model and state transitions.
5. Define authorization and RLS.
6. Define validation.
7. Define failure and retry behavior.
8. Implement database/domain behavior.
9. Implement server/API behavior.
10. Implement UI.
11. Add automated tests.
12. Test duplicate/retry/error cases.
13. Review security and financial implications.
14. Update documentation if the contract changes.

# 15. DEVELOPMENT ORDER — P1

Build in dependency order:

1. Foundation and project configuration.
2. Supabase schema and migrations.
3. Authentication and RLS.
4. Profiles and roles.
5. Wallet and ledger.
6. Deposit/payment provider adapters.
7. Exchange-rate processing.
8. Competitions and registrations.
9. Player dashboard.
10. Admin dashboard.
11. Result extraction and settlement.
12. Withdrawals and payout export.
13. Financial reconciliation.
14. Notifications and support.
15. Testing, security, observability and deployment.

Do not implement a later layer by inventing temporary financial logic that contradicts an earlier layer.

# 16. CHANGE CONTROL — P0

Before changing a database field, financial rule, status transition, authorization rule, payment method, payout behavior or competition settlement rule, identify every affected specification and implementation.

A change MUST be propagated consistently across:

- documentation;
- database schema/migrations;
- server/domain logic;
- UI;
- validation;
- RLS/authorization;
- tests;
- audit behavior.

# 17. ERROR HANDLING — P1

Errors MUST be explicit and actionable.

Never silently swallow payment, wallet, settlement, withdrawal, authentication or authorization errors.

User-facing messages MUST avoid exposing secrets, stack traces or sensitive provider information.

Server logs MAY contain diagnostic context but MUST NOT contain payment secrets, authentication tokens or unnecessary sensitive personal data.

# 18. TESTING REQUIREMENTS — P0

Financial and authorization code MUST have tests covering at minimum:

- successful operation;
- invalid input;
- unauthorized access;
- duplicate request;
- provider retry;
- insufficient funds;
- transaction failure;
- concurrent operation where relevant;
- invalid state transition;
- rollback behavior;
- audit record creation where required.

# 19. DEFINITION OF DONE — P0

A feature is complete only when its UI, database schema, domain rules, authorization, RLS, validation, persistence, idempotency, auditability, error handling and tests agree with the documentation.

A screen that merely looks correct is NOT a completed feature.

A mocked payment success is NOT a completed payment feature.

A client-calculated balance is NOT a completed wallet feature.

An AI-generated result without administrator validation is NOT a completed settlement feature.

# 20. AI AGENT BEHAVIOR — P0

The coding agent MUST:

- read before modifying;
- preserve existing correct behavior;
- prefer explicit specifications over assumptions;
- ask for clarification when a P0 ambiguity cannot be resolved safely;
- avoid placeholder logic in financial/security paths;
- avoid claiming an integration works without the required credentials/configuration and tests;
- leave the repository in a buildable/testable state after changes;
- document important implementation decisions.

The coding agent MUST NOT:

- invent missing business rules;
- weaken security to make a feature work;
- bypass RLS for convenience;
- hard-code secrets;
- hard-code provider success responses;
- delete safeguards because they interfere with a UI flow;
- convert an explicit requirement into a suggestion without recording the change.
