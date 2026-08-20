# BG ARENA — ADMIN DASHBOARD SPECIFICATION

# 1. PURPOSE — P0
The admin application is a privileged operational interface. It must expose controlled management and investigation capabilities without bypassing domain safeguards.

# 2. NAVIGATION — P1
Overview; Users; Competitions; Registrations; Settlements; Payments; Wallet/Transactions; Withdrawals; Financial Reconciliation; Notifications; Support; System Controls where permission allows.

# 3. PERMISSIONS — P0
Use granular permissions such as users.view, competitions.manage, registrations.view, payments.view/review/reconcile/retry, settlements.review/execute, withdrawals.review/complete, reconciliation.view, notifications.manage, support.manage and system_controls.manage.

# 4. OVERVIEW — P1
Show operational KPIs, pending deposits, review-required payments, pending withdrawals, active competitions, recent settlement activity and financial warnings. KPIs must derive from authoritative queries.

# 5. PAYMENT OPERATIONS — P0
Admins can inspect deposit reference, player, provider, method, original amount/currency, provider transaction, conversion rate, USD credit, state and linked ledger transaction. Manual review actions require reason and audit event.

# 6. COMPETITION MANAGEMENT — P1
Create/edit/publish/archive competitions, configure registration fields, entry fee, schedule, capacity, prize structure and settlement configuration. Published financial terms should not be silently changed after registrations without explicit policy.

# 7. SETTLEMENT — P0
Import/validate result JSON, preview affected registrations and amounts, require explicit confirmation, execute one atomic settlement, then lock completed settlement against duplicate execution.

# 8. WITHDRAWALS — P0
Review requests, verify reserved funds/history, export payout instructions, record external outcome and complete/release/reverse according to documented state transitions. Do not execute payout APIs.

# 9. AUDIT — P0
Every privileged financial action records actor, action, target/reference, before/after summary where safe, reason when required, timestamp and correlation ID.

# 10. UX — P1
Dangerous actions require explicit confirmation and explain consequences. Financial amounts must show currency. Long tables need filters, pagination and stable sorting.
