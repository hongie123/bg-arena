# ADMIN DASHBOARD

# 1. OVERVIEW — P1

Provide operational summaries for users, competitions, registrations, payments, pending withdrawals, pending settlements and reconciliation.

# 2. USERS — P1

Search/filter users, inspect profile/status/history and perform permitted account actions. Sensitive operations require confirmation and audit logging.

# 3. COMPETITIONS — P1

Create/edit/publish/open/close/start/cancel/complete competitions through valid state transitions. Configure fees, prizes, registration fields and settlement rules.

# 4. REGISTRATIONS — P1

View participants, dynamic registration values, payment state and registration status while minimizing sensitive information.

# 5. SETTLEMENTS — P0

Import structured results, validate, preview calculations, inspect participant mappings and explicitly confirm settlement. Confirmation is high-risk financial activity and must be audited.

# 6. PAYMENTS — P0

Inspect deposits, provider references, status, conversion details and failures. Reconciliation must not mutate immutable ledger history.

# 7. WALLET/TRANSACTIONS — P0

Search transactions and source references. Administrative adjustments require reason, authorization and compensating ledger entry.

# 8. WITHDRAWALS — P0

Review requests, validate history, reserve/release funds according to state, generate payout instructions and record external outcomes. Never execute payout provider APIs here.

# 9. FINANCIAL RECONCILIATION — P0

Calculate player liabilities and group by payment method/status/date; expose discrepancies and unresolved records.

# 10. NOTIFICATIONS/SUPPORT — P1

Manage authorized announcements and support workflows without exposing secrets.

# 11. ADMIN SAFETY — P0

Never put provider secret keys in the browser. Never rely on hidden frontend routes for authorization. Every privileged action is server-authorized and audited.

# 12. AI IMPLEMENTATION DIRECTIVES

## P0

Admin UI is not a security boundary. Server authorization and audit logs are mandatory.

## P1

Use explicit confirmation dialogs for settlements, adjustments, refunds, withdrawals and other irreversible financial actions.
