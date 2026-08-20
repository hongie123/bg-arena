# BG ARENA — PAYMENTS AND DEPOSITS

# 1. PURPOSE — P0
This document governs external deposit processing. It must be read with the technical blueprint, wallet/ledger and exchange-rate documents.

# 2. PROVIDERS — P0
NOWPayments = crypto deposit adapter. CamPay = mobile-money adapter. Current mobile networks = MTN Cameroon and Orange Cameroon. Provider-specific logic remains under infrastructure/payments.

# 3. PAYMENT METHOD CONFIGURATION — P1
Database-driven methods include NOWPAYMENTS_CRYPTO, CAMPAY_MTN_CM and CAMPAY_ORANGE_CM. Store provider, network, country capability, supported currency/asset, enabled state, display order and public description. Never store secrets in user-readable configuration.

# 4. DEPOSIT INTENT — P0
Create an intent before provider payment. It has stable reference, user, target amount/currency, method, provider, state, expiration and idempotency key. No wallet credit occurs at intent creation.

# 5. CRYPTO FLOW — P0
Create deposit → call NOWPayments server-side → attach provider IDs → display payment asset/network/address/instructions → receive webhook or controlled status → authenticate/validate → idempotency → record source amount/asset → determine authoritative USD conversion → ledger credit.

# 6. MOBILE MONEY FLOW — P0
Create deposit → select MTN/Orange Cameroon → CamPay server-side request → display payment instructions → receive/verify provider status → match provider transaction + deposit reference + amount/currency/method → convert XAF→USD → ledger credit.

# 7. GLOBAL MOBILE MONEY UI — P1
The option may be visible internationally. The UI must say direct support currently covers MTN Cameroon and Orange Cameroon only. Never imply global MTN/Orange support. A trusted person in Cameroon may pay if the provider flow permits it; matching remains tied to the player's deposit intent, not payer identity assumptions.

# 8. WEBHOOK SECURITY — P0
Webhook route is server-only. Verify provider signature/authentication as supported. Parse safely. Reject malformed/unauthenticated events. Check provider transaction, deposit reference, expected amount, currency/asset, state and duplicate history before financial mutation.

# 9. STATE MACHINE — P0
CREATED → AWAITING_PAYMENT → PAYMENT_DETECTED → PAYMENT_CONFIRMED → CONVERSION_PENDING → CREDIT_PENDING → COMPLETED. Failure/review states include EXPIRED, FAILED, REVIEW_REQUIRED, PAYMENT_MISMATCH, CONVERSION_FAILED and CREDIT_FAILED.

# 10. AMOUNT POLICY — P0
Requested target USD, provider payment amount, received source amount, provider fees and final USD credit must be distinct fields. Underpayment/overpayment cannot silently become a full target credit; apply configured policy or REVIEW_REQUIRED.

# 11. DUPLICATE PROTECTION — P0
Unique provider transaction identifiers and internal idempotency references must be enforced in both application logic and database constraints.

# 12. TIMEOUT/RETRY — P0
If provider creation times out, reconcile status before retrying. Use stable internal idempotency reference. Do not create two external payments for one user action.

# 13. RECONCILIATION — P1
Admin must trace deposit → provider transaction → source amount → exchange-rate snapshot → financial transaction → ledger entry.

# 14. TEST MATRIX — P0
Test success, duplicate webhook, fake webhook, wrong player, wrong currency, underpayment, overpayment, expired payment, provider timeout, provider outage, conversion failure, retry recovery and one-time ledger credit.
