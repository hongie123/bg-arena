# DEVELOPMENT ROADMAP

## Phase 1 — Foundation

Set up application framework, TypeScript/types where applicable, linting, formatting, environment handling, Supabase project connection, base routing and design system.

## Phase 2 — Database and security

Create migrations for profiles, roles, games, competitions, registration fields, registrations, wallets, ledger, deposits, withdrawals, settlement, notifications, support and audit logs. Add indexes, constraints and RLS.

## Phase 3 — Authentication

Implement email authentication, profile creation, session handling, protected routes and role-based admin access.

## Phase 4 — Wallet

Implement USD wallet, immutable ledger, transaction display, concurrency protection and administrative audit behavior.

## Phase 5 — Deposits

Implement provider abstraction, payment records, webhook/event handling, idempotency and deposit lifecycle. Add exchange-rate processing and USD crediting.

## Phase 6 — Competitions

Implement game catalog, competition CRUD, status lifecycle, dynamic registration fields and player registration.

## Phase 7 — Player application

Build Dashboard, Games/Tournaments, Wallet, Results & History, Notifications, Tutorials, Support and Account/Settings.

## Phase 8 — Admin application

Build Overview, Users, Competitions, Registrations, Settlements, Payments, Wallet/Transactions, Withdrawals, Reconciliation, Notifications and Support.

## Phase 9 — Settlement

Implement result import, schema validation, game-specific adapters/configuration, preview, explicit confirmation, atomic winnings credits and duplicate protection.

## Phase 10 — Withdrawals

Implement request validation, reservation, payout instruction generation, admin workflow, external outcome recording and reconciliation. Keep payout execution outside BG Arena.

## Phase 11 — Notifications/support

Implement reliable in-app notifications, email integration boundary and support tickets.

## Phase 12 — Hardening

Complete tests, security review, RLS review, financial invariant checks, observability, rate limiting, backups/recovery procedures and deployment validation.

## Implementation discipline

Do not build a large visual shell first and postpone data/security correctness. Each phase should leave a coherent, testable system.
