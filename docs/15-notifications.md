# BG ARENA — NOTIFICATIONS

# 1. PURPOSE — P1
Notifications communicate important account, competition and financial events without becoming the source of truth for those events.

# 2. CHANNELS — P1
In-app notifications are primary. Email may be used for important events. Notification delivery state must be separate from the business event itself.

# 3. EVENT TYPES — P1
Registration confirmed; deposit pending/confirmed/failed; competition updates; result posted; winnings credited; withdrawal requested/status changed; support response; security/account notice; admin announcement.

# 4. FINANCIAL NOTIFICATIONS — P0
Only send a “credited” notification after the financial transaction/ledger state is committed. A provider webhook alone is not sufficient.

# 5. IDEMPOTENCY — P0
Business events may be retried. Notification creation/delivery should use stable event keys to avoid duplicate user messages where policy requires exactly-once presentation.

# 6. PRIVACY — P0
Do not include secrets, full payment credentials, provider signatures or unnecessary personal data. Mask sensitive destination information.

# 7. DELIVERY FAILURE — P1
Notification failure must not roll back a successful financial transaction. Record delivery failure and retry separately.

# 8. PLAYER UI — P1
Unread count, list, detail and mark-read operations. Read state is user-specific and cannot alter underlying business event history.

# 9. TESTS — P0
Test duplicate events, failed email delivery, financial commit before notification, authorization and notification preference behavior.
