# BG ARENA — CLAUDE CODEBASE INSTRUCTIONS

## 1. Purpose

This repository contains the authoritative product, architecture, security, database, financial, API, UI, and implementation specification for BG Arena.

Claude and any other coding agent working in this repository MUST read this file first and then read the relevant documents in `docs/` before changing code or architecture.

The goal is to build a production-ready, game-agnostic competitive gaming platform. The documentation is not a loose product description: it is the implementation contract.

## 2. Source of truth

Priority order:

1. Explicit user decisions recorded in the repository documentation.
2. `docs/01-master-platform-specification.md`.
3. The other specialized documents under `docs/`.
4. Existing application code, only where it does not contradict the specification.
5. Reasonable engineering judgment for implementation details not explicitly decided.

Never silently replace an explicit product decision with a more convenient implementation.

If two documents conflict, stop and identify the conflict instead of inventing a resolution. The master specification and explicit user decisions take precedence.

## 3. Non-negotiable architecture

- BG Arena is game-agnostic at the core.
- Do not hard-code PUBG, CODM, Free Fire, or another game into core user, wallet, competition, registration, or navigation models.
- Game-specific fields belong to competition configuration and registration data.
- Supabase is the planned primary backend/database platform.
- Internal monetary accounting is USD-only.
- Local currencies and cryptocurrencies are deposit currencies, not internal wallet currencies.
- Every deposit must preserve the original currency/asset, original amount, exchange rate used, USD amount credited, provider reference, and lifecycle status.
- Exchange-rate retrieval must happen at deposit processing time through the configured rate source.
- Cameroon launch payment methods include MTN Mobile Money and Orange Money.
- Crypto deposits are supported through the selected crypto payment provider integration.
- The platform must clearly distinguish Cameroon-only mobile-money methods from international methods.
- Authentication is email-based. A phone number may be collected as profile/contact information but is not the authentication factor.
- BG Arena does not directly execute player payouts.
- Withdrawals are validated and funds are reserved in BG Arena, after which an authorized administrator receives a payout instruction for the separate private payout application.
- Payout-provider secret credentials must never be stored in the public BG Arena application.
- Financial operations must be auditable and idempotent.
- Balance changes must be ledger-backed; never mutate a balance without a corresponding auditable transaction.
- Admin actions affecting money, settlement, competition status, or user access require explicit authorization and audit logging.

## 4. Financial safety rules

Treat financial code as protected code.

Never:

- use floating-point arithmetic for money;
- silently round monetary values;
- credit a wallet twice for one provider event;
- settle a competition twice;
- release reserved withdrawal funds without an explicit state transition;
- allow a client to choose its own credited USD amount;
- trust client-supplied exchange rates;
- expose provider secrets to browser/client code;
- execute payout-provider calls from the public client;
- delete financial history to correct an error.

Use integer minor units or an exact decimal strategy consistently. Every financial mutation must have an immutable audit trail and idempotency protection.

## 5. Security

Use Supabase Auth for email authentication and enforce authorization through server-side checks and database Row Level Security where appropriate.

Never expose service-role credentials in frontend code. Never commit `.env` files or secrets. Public environment variables must contain only values safe for the browser.

Admin privileges must be role-based and verified server-side. The UI is not a security boundary.

## 6. Implementation behavior

Before implementing a feature:

1. Identify the relevant specification document.
2. Read its requirements and dependencies.
3. Inspect the current codebase.
4. Implement the smallest coherent production-ready change.
5. Preserve existing contracts unless the specification explicitly changes them.
6. Add validation and error handling.
7. Add or update tests.
8. Check security and financial implications.
9. Update documentation when the implementation changes an established contract.

Do not create placeholder implementations that look complete. If an external provider cannot be safely implemented without credentials or confirmed API details, implement a clean provider boundary and explicit configuration/error path rather than inventing API behavior.

## 7. UI principles

The player experience should be simple, responsive, lightweight, and suitable for the initial Cameroon market. Do not introduce unnecessary complexity.

Player navigation is centered around:

- Dashboard
- Games / Tournaments
- Wallet
- Results & History
- Notifications
- Tutorials
- Support
- Account / Settings

Admin navigation includes:

- Overview
- Users
- Competitions
- Registrations
- Settlements
- Payments
- Wallet / Transactions
- Withdrawals
- Financial Reconciliation
- Notifications
- Support

Player profile presentation should not require stored avatar images. The default profile representation is the user's capitalized first initial with an assigned background color.

## 8. Competition principle

A competition defines its own game, rules, registration requirements, entry fee, capacity, schedule, prize structure, and settlement configuration.

Permanent player profiles must not contain game-specific competitive identifiers that only apply to one game. Game-specific identifiers are collected when registering for the relevant competition.

## 9. Settlement principle

Settlement follows a controlled workflow:

match/result evidence -> result extraction -> structured result data -> admin import/review -> validation -> settlement calculation -> explicit admin confirmation -> atomic ledger transactions -> final settlement.

Where AI is used for result extraction, AI output is untrusted input. BG Arena must validate syntax, schema, competition identity, player identity, numeric values, duplicates, and business rules before money is affected.

## 10. Development order

Build in dependency order, not by visual convenience:

1. repository/project foundation;
2. environment configuration;
3. Supabase schema and migrations;
4. authentication and authorization;
5. core user/profile system;
6. wallet and immutable ledger;
7. deposits and payment-provider boundaries;
8. currency/exchange-rate processing;
9. competitions and registrations;
10. player dashboard;
11. admin dashboard;
12. settlement and result processing;
13. withdrawals and payout export;
14. reconciliation;
15. notifications/support;
16. testing, security hardening, observability and deployment.

## 11. Required reading

Read these documents as needed, but the following are the principal implementation contracts:

- `docs/01-master-platform-specification.md`
- `docs/02-product-requirements.md`
- `docs/03-system-architecture.md`
- `docs/04-supabase-database-schema.md`
- `docs/05-authentication-and-user-management.md`
- `docs/06-player-dashboard.md`
- `docs/07-admin-dashboard.md`
- `docs/08-games-competitions-and-tournaments.md`
- `docs/09-registration-system.md`
- `docs/10-wallet-and-ledger.md`
- `docs/11-payments-and-deposits.md`
- `docs/12-currency-and-exchange-rates.md`
- `docs/13-withdrawals-and-payouts.md`
- `docs/14-settlement-engine.md`
- `docs/15-result-extraction-and-ai.md`
- `docs/16-notifications.md`
- `docs/17-support-system.md`
- `docs/18-security-and-permissions.md`
- `docs/19-api-and-integration-contracts.md`
- `docs/20-environment-and-deployment.md`
- `docs/21-testing-and-quality-assurance.md`
- `docs/22-error-handling-and-edge-cases.md`
- `docs/23-development-roadmap.md`

## 12. Forbidden assumptions

Do not invent:

- payment-provider credentials;
- production API endpoints;
- fee schedules;
- exchange-rate providers when not configured;
- legal claims or regulatory status;
- payout execution capabilities inside BG Arena;
- game-specific permanent profile fields;
- hidden fees;
- automatic administrative actions that were specified as manual;
- financial values supplied by the client.

When something is intentionally manual, keep it manual unless the user explicitly changes the requirement.

## 13. Definition of done

A feature is not complete merely because its UI renders. It is complete when its data model, authorization, validation, business logic, error handling, auditability, tests, loading/empty/error states, and documentation are coherent with this specification.

For financial functionality, completion additionally requires idempotency, immutable transaction history, authorization, reconciliation behavior, and explicit state transitions.
