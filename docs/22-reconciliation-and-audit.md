# BG ARENA — RECONCILIATION AND AUDIT

# 1. PURPOSE — P0
Reconciliation proves that external payment evidence, internal deposits, exchange-rate evidence, financial transactions, ledger entries and wallet balances agree.

# 2. DEPOSIT TRACE — P0
Player → deposit reference → payment method → provider transaction → source amount/currency → rate snapshot → USD amount → financial transaction → ledger entry → wallet.

# 3. RECONCILIATION STATES — P1
MATCHED, MISMATCHED, MISSING_PROVIDER_EVENT, MISSING_LEDGER, DUPLICATE, REVIEW_REQUIRED, RESOLVED. Unknown state must not be hidden as success.

# 4. ADMIN SCREEN — P1
Filters: provider, network, payment method, status, player, date range, currency, deposit reference and provider reference. Detail view shows every linked record necessary for investigation.

# 5. AUDIT LOG — P0
Audit records actor, action, target/reference, timestamp, reason where required, correlation ID and safe metadata. Audit logs are not a replacement for financial ledger entries.

# 6. MANUAL CORRECTIONS — P0
Never edit historical ledger facts to “fix” reconciliation. Create compensating transactions and an audit event explaining the correction.

# 7. BALANCE RECONCILIATION — P0
Compare ledger-derived liabilities and reserved amounts against expected wallet totals. Any discrepancy becomes an operational warning requiring investigation.

# 8. PROVIDER RECONCILIATION — P1
Provider transaction records can be compared with deposit records. Provider success without internal credit requires investigation before any credit is created.

# 9. PAYMENT METHOD REPORTING — P1
Support grouping by NOWPayments/CamPay, MTN/Orange, source currency, date and status. Internal totals remain USD while source amounts remain visible for evidence.

# 10. TESTS — P0
Create deliberate duplicate, missing ledger, mismatched amount and conversion discrepancy fixtures and verify the reconciliation system detects them.
