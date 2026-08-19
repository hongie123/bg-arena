# BG ARENA — TECHNICAL ARCHITECTURE & DEVELOPMENT BLUEPRINT

## Purpose

This document translates the product requirements into an engineering plan. It complements `CLAUDE.md` and the master specification.

## Backend

Use Supabase/PostgreSQL as the primary data layer. Supabase Auth handles email authentication. Row Level Security protects user-owned data. Server-side functions or backend routes perform privileged operations and external integrations.

## Frontend

The frontend should use a component-based architecture with protected player/admin areas, reusable forms, tables, dialogs, notifications and financial displays. The exact framework can follow the selected project stack, but business rules must not live only in UI components.

## Service boundaries

Recommended services/modules:

- auth/profile service;
- competition service;
- registration service;
- wallet service;
- payment service;
- exchange-rate service;
- withdrawal service;
- settlement service;
- notification service;
- support service;
- audit service.

## Database-first development

Implement migrations and constraints before building dependent UI. The schema must enforce important invariants in addition to application validation.

## Financial transaction pattern

For a wallet-affecting operation:

1. authenticate caller/service;
2. authorize operation;
3. validate source entity;
4. acquire appropriate concurrency protection;
5. check current state/balance;
6. write immutable ledger transaction and related state change atomically;
7. commit;
8. emit non-critical notifications after successful commit.

## Provider adapter pattern

External providers should be isolated behind interfaces. The domain layer should consume normalized statuses rather than provider-specific status strings.

Example conceptual interface:

- `createDepositPayment()`;
- `getDepositStatus()`;
- `verifyWebhook()`;
- `getExchangeRate()`.

The exact implementation depends on the provider's official API contract and credentials available at development time.

## State machines

Use explicit state transitions for payments, competitions, registrations, withdrawals and settlements. Do not allow arbitrary status updates from the client.

## Data ownership

Player-owned records are readable by that player according to RLS. Administrative records are restricted to authorized staff. Financial transaction history is never user-editable.

## Code organization

Keep UI, domain logic, database access and external-provider code separated enough that a provider can be replaced without rewriting financial logic.

## Performance

Prefer indexed queries, pagination, server-side filtering and summarized dashboard queries. Do not load an entire transaction history into the browser.

## Audit correlation

High-risk operations should carry a correlation/request ID through API logs, audit records and relevant domain records.

## Build strategy

Implement vertical slices rather than isolated mock screens. For example, complete authentication end-to-end before building dashboard data that depends on it; complete wallet ledger before building deposit UI.

## No fake completeness

Do not replace unavailable provider functionality with fake success responses in production code. Development mocks must be clearly isolated and impossible to mistake for production integrations.
