# BG ARENA — TECHNICAL ARCHITECTURE & DEVELOPMENT BLUEPRINT

# 1. DOCUMENT AUTHORITY — P0

Version: Technical Architecture v2.0. Status: Development Blueprint. This document defines HOW BG Arena is built. The Master Platform Specification defines WHAT BG Arena is. Neither may silently contradict the other.

# 2. TECHNOLOGY STACK — P0

Frontend: Next.js, React, TypeScript. Backend: Next.js server-side services. Database/backend platform: Supabase PostgreSQL. Authentication: Supabase Auth with Google and email identity; phone authentication disabled. Crypto deposits: NOWPayments. Mobile Money: CamPay, MTN Cameroon and Orange Cameroon. Currency normalization: centralized exchange-rate service. Version control: Git/GitHub. AI development: Antigravity + Claude.

# 3. CORE ARCHITECTURE — P0

BG Arena is game-agnostic and USD-denominated. External payments enter through provider adapters, are independently verified, converted into USD, and credited through the immutable financial ledger. Payout execution is outside BG Arena.

# 4. PAYMENT ARCHITECTURE — P0

## 4.1 Providers — P0
NOWPayments handles crypto deposits. CamPay handles MTN Cameroon and Orange Cameroon Mobile Money. Provider capabilities must be configuration-driven; do not permanently hardcode a cryptocurrency list or unsupported country/network.

## 4.2 Global Mobile Money presentation — P0
The Mobile Money option may be displayed internationally, but the UI must explicitly state that direct support is currently limited to MTN Cameroon and Orange Cameroon. It must not imply MTN/Orange global coverage.

## 4.3 Payment identity — P0
The Mobile Money phone number used for a payment may belong to a trusted third party. Deposit ownership comes from the verified BG Arena deposit intent and provider reference, not from assuming the payer phone equals the account phone.

# 5. DEPOSIT PIPELINE — P0

Player chooses USD target → BG Arena creates deposit intent → provider payment is created → player pays → provider confirms → webhook/status is authenticated → provider transaction is matched → conversion is determined → exchange-rate snapshot is stored → USD financial transaction is created → immutable ledger credit is written atomically → wallet reflects USD → notification/reconciliation data is persisted.

## 5.1 Deposit states — P0
Recommended states: `CREATED`, `PENDING`, `AWAITING_PAYMENT`, `PAYMENT_DETECTED`, `PAYMENT_CONFIRMED`, `CONVERSION_PENDING`, `CREDIT_PENDING`, `COMPLETED`, `EXPIRED`, `FAILED`, `CANCELLED`, `REVIEW_REQUIRED`. State transitions must be centralized and validated.

## 5.2 Deposit records — P0
A deposit preserves deposit reference, user, payment method, provider, provider transaction/reference, requested amount/currency, received amount/currency, USD amount, exchange rate, rate source/timestamp/reference, status, metadata and lifecycle timestamps.

# 6. CURRENCY ARCHITECTURE — P0

All completed deposits normalize into USD. Fiat uses the exchange-rate service. Crypto should prefer applicable provider-confirmed payment data where it establishes the financial value, while still storing BG Arena's own USD accounting snapshot. Failed rate retrieval must never produce a guessed value.

## 6.1 Rate abstraction — P0
Use `ExchangeRateProvider` behind `ExchangeRateService`. Support primary/fallback providers only when explicitly configured and record which source was used. Display estimates and settlement rates are separate concepts.

# 7. FINANCIAL ARCHITECTURE — P0

Core tables: `wallet_accounts`, `ledger_entries`, `financial_transactions`, `financial_reservations`, `deposits`, `payment_methods`, `payment_provider_transactions`, `payment_events`, `exchange_rate_snapshots`. The ledger is append-only and authoritative. Available balance is derived centrally from credits, debits and active reservations.

# 8. COMPETITIONS — P0

Competitions remain game-agnostic. Use `competitions`, `competition_custom_fields`, `competition_custom_field_options`, `competition_registrations`, and `competition_registration_values`. Entry fees are USD and wallet deductions are atomic.

# 9. SETTLEMENT — P0

External result system → settlement JSON → schema/identity/eligibility validation → financial calculation → admin preview → explicit approval → atomic ledger mutation → audit and duplicate protection. AI output is never financial authority.

# 10. WITHDRAWALS — P0

Player request → validation → USD reservation → admin review → payout JSON export → private payout application → manual result return → complete/release/reconcile. BG Arena does not execute payout APIs.

# 11. DIRECTORY CONTRACT — P1

Application ownership follows the documented feature/infrastructure/financial split. Payment adapters live under `infrastructure/payments/nowpayments` and `infrastructure/payments/campay`; exchange-rate logic lives under `infrastructure/exchange-rates`; ledger logic lives under `financial/ledger`; deposits own user-facing deposit workflows but never directly write the ledger.

# 12. API CONTRACT — P0

Representative routes: `/api/deposits`, `/api/deposits/[id]`, provider create/status routes, `/api/payments/webhooks/nowpayments`, `/api/payments/webhooks/campay`, and `/api/exchange-rates`. Exact route names may evolve, but server-only financial boundaries may not.

# 13. SECURITY — P0

Public browser variables may contain only public Supabase configuration. Service-role, provider API keys, webhook secrets, exchange-rate secrets and email secrets are server-only. RLS protects records and server authorization protects privileged financial operations.

# 14. TESTING — P0

Mandatory payment tests include successful crypto, MTN and Orange deposits; duplicate webhook; duplicate provider transaction; rate outage; rate recovery; underpayment; overpayment; wrong currency; unmatched player; unauthenticated webhook; provider timeout; idempotent payment creation; and reconciliation.

# 15. DEVELOPMENT PHASES — P1

1. Foundation. 2. Authentication. 3. Competitions. 4. Financial foundation. 5. Payment infrastructure. 6. Currency infrastructure. 7. Deposits. 8. Settlements. 9. Withdrawals. 10. Reconciliation. 11. Administration. 12. Hardening.

# 16. FINAL ARCHITECTURAL RULE — P0

No external payment becomes wallet money until BG Arena establishes authenticity, deposit ownership, uniqueness, valid conversion and ledger recording. Once credited, the USD amount is historical and immutable except through explicit compensating financial operations.
