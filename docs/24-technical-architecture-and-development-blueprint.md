# BG ARENA — TECHNICAL ARCHITECTURE & DEVELOPMENT BLUEPRINT

# 1. DOCUMENT STATUS — P0
Version: Technical Architecture v2.0+. This is the authoritative implementation blueprint and incorporates the approved architecture for USD-only accounting, NOWPayments crypto deposits, CamPay MTN/Orange Cameroon mobile money, centralized exchange rates, immutable ledger accounting, game-agnostic competitions and external/manual payouts.

# 2. STACK — P0
Frontend: Next.js + React + TypeScript. Backend: Next.js server-side services and Supabase. Database: Supabase PostgreSQL. Authentication: Supabase Auth with Google/email identity. Crypto deposits: NOWPayments. Mobile money: CamPay with MTN Cameroon and Orange Cameroon. Version control: Git/GitHub. AI development: Claude/Antigravity.

# 3. ARCHITECTURAL PRINCIPLES — P0
Game-agnostic core. USD-only internal wallet. External payment currencies are normalized into USD. Immutable ledger is financial source of truth. Provider records and accounting records are separate. UI is not financial authority. AI extraction is not settlement authority. BG Arena is not the payout executor.

# 4. COMPLETE PAYMENT PIPELINE — P0
PLAYER → DEPOSIT INTENT → PAYMENT PROVIDER → VERIFIED EXTERNAL EVENT → PROVIDER TRANSACTION → EXCHANGE-RATE SERVICE → USD VALUE → FINANCIAL TRANSACTION → LEDGER ENTRY → USD WALLET → NOTIFICATION → RECONCILIATION.

# 5. PAYMENT METHODS — P0
Database-driven methods include NOWPAYMENTS_CRYPTO, CAMPAY_MTN_CM and CAMPAY_ORANGE_CM. Crypto availability must come from provider/configuration rather than permanent hardcoded assumptions. Crypto assets and networks are separate concepts. Mobile-money capability is currently Cameroon-specific even if the UI is globally visible.

# 6. DEPOSIT DATA — P0
Minimum deposit data: id, deposit_reference, user_id, payment_method_id, provider, provider_transaction_id, provider_payment_reference, requested_amount, requested_currency, received_amount, received_currency, usd_amount, exchange_rate, exchange_rate_source, exchange_rate_timestamp, exchange_rate_reference, status, payment_metadata, created_at, confirmed_at, failed_at, expires_at and idempotency_key as required.

# 7. DEPOSIT STATE MACHINE — P0
CREATED → AWAITING_PAYMENT → PAYMENT_DETECTED → PAYMENT_CONFIRMED → CONVERSION_PENDING → CREDIT_PENDING → COMPLETED. Failure/review states: EXPIRED, FAILED, PAYMENT_MISMATCH, CONVERSION_FAILED, CREDIT_FAILED, REVIEW_REQUIRED. Invalid transitions are rejected centrally.

# 8. NOWPAYMENTS — P0
Use a server-side adapter with client/service/types/webhook/validation modules. Attach the BG Arena deposit reference to the provider order/reference when supported. Verify provider callback/status, match provider IDs, enforce idempotency and only then proceed to conversion/accounting.

# 9. CAMPAY — P0
Use a server-side adapter with client/service/types/webhook/validation modules. Current supported networks are MTN Cameroon and Orange Cameroon. Never claim that the gateway supports those networks globally. Provider credential names must follow the actual provider contract rather than invented names.

# 10. MOBILE MONEY IDENTITY — P0
The payer's mobile-money phone number need not equal the player's profile phone number. A trusted person may pay on the player's behalf when the provider flow permits it. The deposit remains linked to the authenticated player's deposit intent. Matching relies on verified provider/deposit references and payment evidence, not payer-phone equality.

# 11. EXCHANGE RATE — P0
Central service only. No page calls a rate API directly. Store rate snapshot and source when used. Do not guess when unavailable. Display estimates are not automatically authoritative settlement rates. Historical completed credits never change with future rates.

# 12. WALLET/LEDGER — P0
Each player has one primary USD wallet. Financial transactions and ledger entries are created atomically. Available balance is centralized. Reservations support withdrawals. Corrections are compensating transactions, not edits/deletes of historical ledger rows.

# 13. COMPETITIONS — P1
Competition data includes game/reference, format, title, rules, schedule, registration window, capacity, entry fee USD, prize structure, status and dynamic fields. Permanent profiles remain game-neutral.

# 14. REGISTRATION — P0
Registration validates competition state, dynamic fields, capacity, duplicate registration and available USD. Entry fee debit and registration confirmation are atomic. Game-specific IDs belong to registration values.

# 15. SETTLEMENT — P0
External/AI result input is untrusted. Validate schema, competition, registrations, numeric values, duplicates and settlement configuration. Require admin preview and explicit confirmation. Execute atomic USD credits. Prevent duplicate settlement.

# 16. WITHDRAWAL — P0
Request → validate → reserve USD → create withdrawal → admin review → payout JSON → private payout application → external outcome → controlled reconciliation. BG Arena never executes payout-provider APIs.

# 17. DATABASE — P0
Core tables: profiles, admin_roles, competitions, competition_custom_fields/options, competition_registrations/values, wallet_accounts, ledger_entries, financial_transactions, financial_reservations, deposits, payment_methods, payment_provider_transactions, payment_events, exchange_rate_snapshots, settlements, settlement_items, withdrawals, withdrawal_events, notifications, support_conversations/messages, disputes, audit_logs, system_controls.

# 18. SECURITY — P0
RLS is mandatory. Player writes to financial tables are prohibited except through approved server-side use cases. Secrets remain server-side. Webhooks are authenticated. Dynamic inputs are schema-validated. Admin permissions are explicit and audited.

# 19. API BOUNDARY — P0
API routes call domain services. Provider APIs are infrastructure. Client cannot call provider secrets or ledger mutations. Webhook endpoints are independent of browser sessions and use provider authentication.

# 20. DIRECTORY CONTRACT — P1
Use `app/`, `features/`, `shared/`, `infrastructure/`, `financial/`, `database/`, `tests/`, `docs/`. Payment adapters live under infrastructure/payments; exchange rates under infrastructure/exchange-rates; ledger/balance/reservations under financial.

# 21. SYSTEM CONTROLS — P0
Support emergency switches such as deposits_enabled, crypto_deposits_enabled, mobile_money_deposits_enabled, campay_mtn_enabled, campay_orange_enabled and nowpayments_enabled. Changes are permissioned and audited.

# 22. FINANCIAL LIMITS/FEES — P1
Support configurable minimum/maximum deposit, daily/monthly limits and explicit fee policy. The code must not implicitly choose whether a target USD amount means gross payment or net wallet credit; that business policy must be configured/documented.

# 23. RECONCILIATION — P0
Every completed deposit must be reconstructable from player → deposit → payment method → provider transaction → source amount → rate snapshot → financial transaction → ledger → wallet. Discrepancies become explicit review states.

# 24. DEVELOPMENT ORDER — P0
Foundation → database/RLS → authentication → profiles → wallet/ledger → payment abstraction → NOWPayments/CamPay → webhook/idempotency → exchange rates → deposits → competitions/registration → player/admin UI → settlement → withdrawals → reconciliation → notifications/support → security/testing/deployment.

# 25. PRODUCTION DEFINITION OF DONE — P0
Provider integration works; secrets protected; payment creation/status works; callbacks verified; duplicate events rejected; source currency preserved; conversion snapshot preserved; USD credit atomic; wallet ledger-backed; notifications/reconciliation work; tests pass; RLS verified; Git diff reviewed; documentation matches implementation.

# 26. ARCHITECTURAL CONFLICT PROCEDURE — P0
If implementation conflicts with this blueprint: stop; identify conflict; explain impact; propose alternatives; obtain explicit approval; update master/technical/domain documents; then implement. No AI agent may silently redefine BG Arena.
