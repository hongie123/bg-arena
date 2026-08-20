# NOTIFICATIONS

# 1. CHANNELS — P1

Primary channels are in-app notifications and email where configured. SMS is not an authentication requirement.

# 2. EVENTS — P1

Notifications may be generated for registration, payment, deposit credit, competition changes, results, winnings, withdrawal requests/status changes, support responses and administrative announcements.

# 3. RELIABILITY — P0

Financial operations never depend on notification delivery. A failed email/notification cannot roll back a successful financial transaction.

# 4. IDEMPOTENCY — P0

Retries of the same domain event must not create unlimited duplicate notifications.

# 5. PREFERENCES — P2

Notification preferences remain separate from financial state.

# 6. AI IMPLEMENTATION DIRECTIVES

## P0

Notifications are secondary side effects, not financial authority.

## P1

Use durable event references and retry-safe delivery processing.
