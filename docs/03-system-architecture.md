# BG ARENA — SYSTEM ARCHITECTURE

## 1. Architectural layers

BG Arena should be separated into:

1. presentation/client;
2. authenticated application/API layer;
3. domain/business services;
4. persistence/database;
5. external provider adapters;
6. asynchronous/event processing where required;
7. administration tooling.

The client must not be treated as a trusted business layer.

## 2. Domain boundaries

Primary domains:

- identity and profiles;
- competitions;
- registrations;
- wallet/ledger;
- payments/deposits;
- exchange rates;
- withdrawals;
- settlement;
- notifications;
- support;
- administration;
- audit.

Each domain should expose explicit service boundaries instead of allowing unrelated UI components to mutate database records directly.

## 3. Request flow

Typical authenticated request:

client -> authentication -> server-side authorization -> domain validation -> transaction/business operation -> database -> response.

External payment flow:

provider -> verified webhook/controlled event endpoint -> event validation -> idempotency -> payment record -> exchange-rate processing -> ledger transaction -> notification.

## 4. Transaction boundary

Money-changing operations must be atomic. A wallet credit must not succeed while its corresponding transaction record fails, and an entry-fee debit must not succeed without the associated registration/payment state being coherent.

Use PostgreSQL transactions/functions or a secure server-side transaction mechanism appropriate to Supabase.

## 5. Event/idempotency architecture

Every provider event that can cause financial mutation must have a durable unique external event/provider reference.

Repeated delivery must return the existing result and must never create a second credit/debit.

## 6. Separation of public and private services

The public BG Arena application handles user-facing platform operations.

The private payout application is outside this repository's trusted public runtime and stores payout-provider secrets. BG Arena communicates with it through manually transferred or otherwise explicitly authorized payout instructions as defined by the payout specification.

## 7. Game adapter architecture

The generic platform should represent a competition and its registration/settlement configuration without knowing game-specific fields.

Game adapters/configurations may define:

- registration fields;
- result fields;
- validation rules;
- settlement rules;
- scoring calculations.

The core wallet and user domains consume only normalized settlement outputs.

## 8. Database access

Frontend code should access only data allowed by RLS and application contracts. Privileged operations should run server-side.

Never ship the Supabase service-role key to the browser.

## 9. Observability

Production implementation should provide structured logging around:

- payment events;
- wallet transactions;
- settlement attempts;
- withdrawal state transitions;
- authorization failures;
- provider errors;
- webhook verification failures.

Logs must not contain secrets or unnecessary sensitive data.

## 10. Failure philosophy

External providers are unreliable dependencies. The platform must use explicit pending/failed/retryable states rather than assuming every network call succeeds.

A provider timeout must not automatically mean that a payment failed. Reconciliation and provider confirmation must determine final state.
