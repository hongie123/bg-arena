# DEVELOPMENT ROADMAP

# 1. PHASE 1 — FOUNDATION — P1

Set up application framework, TypeScript/types, linting, formatting, environment handling, Supabase connection, base routing and design system.

# 2. PHASE 2 — DATABASE AND SECURITY — P0

Create migrations for profiles, roles, games, competitions, registration fields, registrations, wallets, ledger, deposits, withdrawals, settlement, notifications, support and audit logs. Add constraints, indexes and RLS.

# 3. PHASE 3 — AUTHENTICATION — P0

Implement email authentication, profile creation, session handling, protected routes and role-based admin access.

# 4. PHASE 4 — WALLET — P0

Implement USD wallet, immutable ledger, transaction display, concurrency protection and audit behavior.

# 5. PHASE 5 — DEPOSITS — P0

Implement provider abstraction, payment records, verified events, idempotency, deposit lifecycle, exchange-rate processing and USD crediting.

# 6. PHASE 6 — COMPETITIONS — P1

Implement game catalog, competition CRUD, lifecycle, dynamic registration fields and registration.

# 7. PHASE 7 — PLAYER APPLICATION — P1

Build Dashboard, Games/Tournaments, Wallet, Results & History, Notifications, Tutorials, Support and Account/Settings.

# 8. PHASE 8 — ADMIN APPLICATION — P1

Build Overview, Users, Competitions, Registrations, Settlements, Payments, Wallet/Transactions, Withdrawals, Reconciliation, Notifications and Support.

# 9. PHASE 9 — SETTLEMENT — P0

Implement result import, schema validation, game-specific adapters/configuration, preview, explicit confirmation, atomic winnings credits and duplicate protection.

# 10. PHASE 10 — WITHDRAWALS — P0

Implement request validation, reservation, payout instruction generation, admin workflow, external outcome recording and reconciliation. Keep payout execution outside BG Arena.

# 11. PHASE 11 — NOTIFICATIONS/SUPPORT — P1

Implement reliable in-app notifications, email integration boundary and support tickets.

# 12. PHASE 12 — HARDENING — P0

Complete tests, security/RLS review, financial invariant checks, observability, rate limiting, backups/recovery and deployment validation.

# 13. IMPLEMENTATION DISCIPLINE — P0

Do not build a large visual shell first and postpone data/security correctness. Each phase should leave a coherent, testable system.

# 14. AI IMPLEMENTATION DIRECTIVES

## P0

Do not skip a dependency phase merely to make a UI appear complete.

## P1

A phase is complete only when its data, authorization, domain logic, UI, tests and failure behavior are coherent.
