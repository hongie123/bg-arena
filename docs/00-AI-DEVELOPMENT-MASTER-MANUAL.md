# BG ARENA — AI DEVELOPMENT MASTER MANUAL

## 1. DOCUMENT STATUS

**Priority: P0 — AUTHORITATIVE IMPLEMENTATION CONTRACT**

This document is the operating manual for any AI coding agent (Claude Code, Cursor, Codex, Gemini, Antigravity, or another engineering agent) used to build BG Arena.

The agent MUST read `CLAUDE.md`, this document, and the relevant domain specification before writing or changing code.

This repository is not a collection of suggestions. It is the implementation specification for a real production system.

## 2. PRIORITY SYSTEM

### P0 — NON-NEGOTIABLE

Rules marked P0 must never be violated for convenience, speed, UI preference, or framework limitations.

Examples:

- financial correctness;
- authorization and RLS;
- USD-only internal accounting;
- email authentication;
- no direct payout execution in BG Arena;
- no provider secrets in client code;
- idempotency for external financial events;
- immutable financial history;
- game-agnostic core architecture.

### P1 — REQUIRED PRODUCT BEHAVIOR

P1 requirements define expected user/admin behavior and must be implemented unless an explicit user decision changes them.

### P2 — IMPLEMENTATION GUIDANCE

P2 requirements describe preferred implementation patterns. An agent may choose an equivalent implementation only when it preserves the same behavior and security properties.

### P3 — OPTIONAL / FUTURE

P3 items may be deferred and must not be silently presented as completed functionality.

## 3. SOURCE-OF-TRUTH HIERARCHY

When requirements conflict, use this order:

1. Explicit user decisions in the project conversation.
2. `CLAUDE.md`.
3. This master manual.
4. `docs/01-master-platform-specification.md`.
5. Specialized domain documents.
6. Existing code.
7. Engineering judgment.

If a conflict affects money, authentication, permissions, or data integrity, STOP rather than guessing.

## 4. PRODUCT IDENTITY

BG Arena is a game-agnostic competitive gaming platform. Cameroon is the initial market. The architecture must support international expansion without replacing the core financial or user model.

The application has two security domains:

- **Player application:** discovery, registration, wallet, history, notifications, tutorials, support and account management.
- **Admin application:** platform operations, users, competitions, payments, settlements, withdrawals, reconciliation, notifications and support.

## 5. CORE INVARIANTS

### 5.1 Currency invariant

All internal monetary values are USD. Deposits may arrive as XAF, another fiat currency, or supported crypto assets. The original denomination is preserved for audit, but the credited wallet value is USD.

### 5.2 Wallet invariant

A wallet balance is a consequence of the ledger. Never create a second independent source of truth by allowing arbitrary client-side balance mutations.

### 5.3 Payment invariant

A provider event can credit a wallet at most once. Provider references and event IDs must be unique/idempotent.

### 5.4 Settlement invariant

A competition settlement can be finalized at most once. Repeated requests must return the existing outcome or a controlled conflict.

### 5.5 Withdrawal invariant

A withdrawal reserves available funds before it becomes pending. Reserved funds cannot simultaneously be spent by another operation.

### 5.6 Payout invariant

BG Arena generates payout instructions but does not execute payout-provider calls. Payout secrets belong only to the separate private payout application.

### 5.7 Authentication invariant

Email is the authentication factor. Phone number is optional contact/payment information and must not become an authentication mechanism.

### 5.8 Game-agnostic invariant

Do not add PUBG/CODM/Free Fire-specific columns to the permanent player profile or generic wallet. Game-specific identifiers and metrics belong to competition registration/configuration and settlement adapters.

## 6. REQUIRED DEVELOPMENT BEHAVIOR

Before coding any feature:

1. Identify the feature's specification file.
2. Read dependent specifications.
3. Inspect existing files and database migrations.
4. Identify authorization requirements.
5. Identify financial implications.
6. Define validation and failure states.
7. Implement server-side business rules.
8. Implement UI states only after the domain behavior is safe.
9. Add tests for normal, invalid, duplicate and concurrent cases.
10. Update documentation if behavior or schema changes.

Never build a fake UI that implies a backend capability that does not exist.

## 7. ARCHITECTURE RULES

### Frontend

Use a typed, component-based application architecture. Keep presentation, domain operations, data access and external provider adapters separated.

### Backend

Use Supabase/PostgreSQL as the planned primary backend. Use database constraints/RLS plus trusted server-side operations for sensitive mutations.

### Provider adapters

Payment providers, exchange-rate services, email services and other external integrations must be isolated behind adapters. Provider-specific payloads must not leak throughout the domain model.

### Storage

Do not store large video/audio/image binaries in ordinary database rows. Store object-storage references and metadata.

## 8. FINANCIAL IMPLEMENTATION RULES

Never use binary floating point for financial calculations. Use integer minor units or an exact decimal representation consistently.

Every financial operation must have:

- authenticated actor/context;
- authorization check;
- domain validation;
- unique reference/idempotency key;
- explicit state transition;
- ledger impact where money changes;
- audit event;
- failure-safe transaction boundary.

Never accept a client-supplied credited USD amount as authoritative.

Never trust a client-supplied exchange rate.

Never delete financial history to correct an error; use a compensating transaction.

## 9. PAYMENT METHODS

### Cameroon launch

- MTN Mobile Money — Cameroon-only.
- Orange Money — Cameroon-only.
- Crypto — international, subject to configured provider/network support.

The UI must clearly tell users that MTN/Orange Mobile Money are Cameroon-only even if the payment method configuration is visible to international users.

Payment-provider credentials are server-side secrets.

## 10. DEPOSIT PROCESSING

The authoritative deposit pipeline is:

`create deposit -> initiate/instructions -> provider event -> authenticate event -> idempotency -> validate amount/status -> capture original denomination -> obtain exchange rate -> calculate USD -> ledger credit -> mark credited -> audit -> notification`

No browser success screen is sufficient evidence of payment.

If exchange-rate retrieval fails, do not guess. Leave the deposit in an appropriate pending/error state and surface an actionable admin state.

## 11. COMPETITIONS

A competition owns its:

- game reference;
- title/description;
- rules;
- schedule;
- registration window;
- capacity;
- entry fee;
- prize structure;
- dynamic registration fields;
- settlement configuration;
- status.

Competition status transitions must be explicit and validated. Do not allow registration into closed/full/cancelled competitions.

## 12. REGISTRATION

Dynamic registration fields are defined by the competition. Examples may include a game username, player ID, team name, UID, server region or other game-specific value.

These values belong to the registration, not the permanent user profile.

Entry-fee charging and successful registration must be atomic from the user's perspective.

## 13. SETTLEMENT

Settlement is a controlled financial operation:

`evidence -> extraction/import -> schema validation -> business validation -> admin review -> calculation -> explicit confirmation -> atomic ledger writes -> final status`

AI extraction is never authoritative.

The system must validate player identities against actual competition registrations and reject duplicate or unknown participants.

For PUBG-like kill/finish settlement, game-specific rules belong to a settlement adapter/configuration. The generic engine only understands normalized results and settlement outputs.

## 14. WITHDRAWALS

The player requests a USD amount and payout destination information. BG Arena validates eligibility, reserves funds and creates a withdrawal record. Admins review and export normalized payout instructions.

The private payout application executes the external payment. BG Arena later records the externally confirmed outcome through the controlled reconciliation workflow.

Do not add payout-provider credentials to this repository.

## 15. SECURITY

Assume every client request can be forged.

RLS and server-side authorization must prevent:

- reading another user's wallet;
- changing another user's profile;
- changing roles;
- creating false credits;
- approving settlements;
- completing withdrawals;
- viewing protected admin data.

Validate all dynamic JSON and registration fields.

## 16. AUDIT LOGGING

Audit events should capture actor, action, target, timestamp, reason/context and safe before/after information. Do not store secrets in audit logs.

Financial and privileged administrative operations require audit events.

## 17. UI RULES

The UI must be responsive, lightweight, accessible and explicit about financial state.

Every asynchronous page needs loading, empty, success and error states.

Money displays must consistently show USD for internal balances and clearly identify external deposit currency where relevant.

Destructive or money-affecting actions require confirmation and should explain the consequence.

## 18. ENVIRONMENT AND SECRETS

Never commit `.env` files containing secrets.

Browser-safe configuration may be public. Service-role keys, provider API keys, webhook secrets, exchange-rate credentials and payout credentials are server-only.

For deployment platforms such as Vercel, configure secrets in project environment settings.

## 19. TESTING STANDARD

A feature is incomplete until its:

- happy path;
- validation path;
- authorization path;
- duplicate/retry path;
- concurrency path where relevant;
- external-provider failure path;
- persistence behavior;
- UI error/loading state

have been considered and tested.

Financial code receives stronger testing than ordinary presentation code.

## 20. AI CODING AGENT CHECKLIST

Before committing:

- [ ] Read relevant specification.
- [ ] No core game-specific hard-coding.
- [ ] No secret committed.
- [ ] No client-authoritative money value.
- [ ] No client-authoritative role/permission.
- [ ] No direct payout execution in BG Arena.
- [ ] Idempotency implemented for external events.
- [ ] Database constraints/RLS considered.
- [ ] Loading/empty/error states exist.
- [ ] Tests cover failure cases.
- [ ] Documentation matches implementation.
- [ ] No TODO pretending to be completed functionality.

## 21. DEFINITION OF DONE

The application is not considered complete because screens exist. A production feature is complete only when the UI, database model, security model, business rules, validation, error handling, audit trail, tests, and documentation agree.
