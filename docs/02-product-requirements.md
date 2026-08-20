# BG ARENA — PRODUCT REQUIREMENTS

# 1. USERS AND ROLES — P1

## Player

A player can create an account, authenticate by email, maintain a profile, browse competitions, register, pay entry fees, participate, view results, receive winnings, deposit funds, request withdrawals, receive notifications, use tutorials and contact support.

## Admin

An authorized administrator manages users, competitions, registrations, payments, settlements, withdrawals, reconciliation, notifications and support.

## System/service roles

Server-side services process authenticated provider events, exchange-rate lookups, settlement operations, notifications and other controlled automation.

# 2. ACCOUNT REQUIREMENTS — P0

Required concepts include unique user ID, email, display/name information, optional phone contact data, account status, protected role and timestamps. Phone number must never become an authentication factor without an explicit product change.

# 3. PLAYER REQUIREMENTS — P1

The player must be able to register/sign in/out, recover access by email, manage profile, browse competitions, inspect rules/schedule/fees/capacity/prizes, register with dynamic game-specific fields, pay entry fees, view USD wallet/transactions, deposit, see source-currency conversion information, view results/winnings, request withdrawals, see withdrawal status, receive notifications, use tutorials/support and manage settings.

# 4. COMPETITION REQUIREMENTS — P1

Admins can configure game, title, description, format, rules, registration window, schedule, participant limit, USD entry fee, USD prize structure, status, dynamic registration fields and settlement configuration.

Status transitions must be explicit and validated:

`DRAFT -> PUBLISHED -> REGISTRATION_OPEN -> REGISTRATION_CLOSED -> IN_PROGRESS -> RESULT_PENDING -> SETTLEMENT_PENDING -> SETTLED -> COMPLETED/CANCELLED`

# 5. REGISTRATION REQUIREMENTS — P0

Validate authentication, account status, competition state, time window, capacity, duplicate registration, dynamic fields, eligibility and entry-fee availability. Registration/payment consistency must be atomic.

# 6. WALLET REQUIREMENTS — P0

Display available/reserved USD and immutable transaction history. The client cannot mutate balances.

# 7. PAYMENT REQUIREMENTS — P1

Clearly show payment method, source amount, estimated/confirmed USD equivalent, applicable fees, status and safe provider reference. MTN Mobile Money and Orange Money must be clearly labelled Cameroon-only.

# 8. ADMIN REQUIREMENTS — P1

Provide operational visibility into users, competitions, registrations, deposits, liabilities, pending withdrawals, pending settlements, failed payment events, reconciliation differences and support workload.

# 9. AUDIT REQUIREMENTS — P0

Sensitive actions retain actor, action, target, timestamp, reason where required and safe before/after information plus a correlation/reference ID.

# 10. UX REQUIREMENTS — P1

Every asynchronous feature needs loading, empty, success and error states. Financial actions require confirmation and transparent amounts, fees, conversion information and status.

# 11. AI IMPLEMENTATION DIRECTIVES

## P0

Do not implement requirements solely as static UI. Every financial/product action must map to a secure server-side domain operation and persistent state.

## P1

Build player and admin flows from the requirements above before adding optional polish.

## P2

Use reusable components, typed validation schemas and explicit state machines for status-driven workflows.
