# BG ARENA — PRODUCT REQUIREMENTS SPECIFICATION

# 1. PRODUCT PURPOSE — P0
BG Arena provides a simple competitive gaming experience: account creation, competition discovery, competition registration, participation information, financial wallet management, results/history, notifications and support.

# 2. PRIMARY ACTORS — P0
## 2.1 Player
Can authenticate, complete profile information, browse eligible competitions, register, fund wallet, view ledger-backed history, receive winnings and request withdrawal.
## 2.2 Admin
Can manage users, competitions, registrations, payment records, settlements, withdrawals, reconciliation, notifications, support and system controls according to assigned permissions.
## 2.3 External provider
NOWPayments or CamPay supplies external payment events. It never becomes the internal financial source of truth.
## 2.4 Private payout operator
Executes payouts outside BG Arena and returns outcomes through the controlled reconciliation process.

# 3. CORE PLAYER REQUIREMENTS — P1
The player must always know account state, wallet balance, competition status, registration status and important notifications. Financial values shown as wallet/accounting values are USD.

# 4. ACCOUNT REQUIREMENTS — P0
Authentication uses Google and/or email identity. Phone number may be collected but cannot authenticate. Profile has no mandatory stored image. Default avatar uses capitalized initial plus assigned/deterministic background.

# 5. COMPETITION REQUIREMENTS — P1
Competitions define game, format, title, rules, schedule, capacity, entry fee, prize structure, status and dynamic registration requirements. Game-specific identifiers belong to registration, not the permanent profile.

# 6. FINANCIAL REQUIREMENTS — P0
Wallet is USD-only. Deposits can originate in supported crypto or external fiat/mobile-money currencies. Every verified deposit is converted into USD and represented by a financial transaction plus ledger entry. Entry fees and winnings are USD.

# 7. PAYMENT REQUIREMENTS — P0
Crypto deposits use NOWPayments. Mobile money uses CamPay with MTN Cameroon and Orange Cameroon. Mobile Money can be visible globally but must clearly state current Cameroon-only network support. A friend's supported Cameroon number may pay a player's deposit; matching is by deposit/provider references and verified transaction information, not by assuming payer phone equals player phone.

# 8. WITHDRAWAL REQUIREMENTS — P0
BG Arena validates and reserves funds, creates a withdrawal request and payout instruction. It never stores payout secrets or executes payout APIs.

# 9. ADMIN REQUIREMENTS — P1
All privileged financial actions require authorization and audit logging. Admin interfaces must expose enough traceability to reconstruct payment → conversion → ledger → wallet.

# 10. NON-FUNCTIONAL REQUIREMENTS — P0
Security, financial correctness, idempotency, RLS, auditability and deterministic state transitions outrank visual polish. P1 performance target is responsive normal usage; P2 optimizations must not weaken correctness.

# 11. ACCEPTANCE PRINCIPLE — P0
A requirement is accepted only when its happy path, failure states, authorization, persistence and tests are defined. “Looks right” is not acceptance.
