# GAMES, COMPETITIONS AND TOURNAMENTS

# 1. GAME-AGNOSTIC MODEL — P0

A game is a catalog entity. A competition references a game but owns its operational configuration. Never add `pubg_id`, `codm_uid` or similar permanent profile fields.

# 2. COMPETITION CONFIGURATION — P1

A competition defines game, format, title/description, rules, registration window, event schedule, capacity, USD entry fee, USD prize structure, dynamic registration fields, settlement configuration and status.

# 3. FORMATS — P1

The model must support tournaments, matches, brackets, leagues and future formats without changing wallet architecture.

# 4. STATUS LIFECYCLE — P0

`DRAFT -> PUBLISHED -> REGISTRATION_OPEN -> REGISTRATION_CLOSED -> IN_PROGRESS -> RESULT_PENDING -> SETTLEMENT_PENDING -> SETTLED -> COMPLETED`

Cancellation paths must define entry-fee/refund consequences.

# 5. PRIZES — P0

Prize amounts are USD. Settlement cannot exceed configured distributable amounts. Platform fees, if introduced, must be explicitly configured and represented.

# 6. ELIGIBILITY — P0

Validate status, timing, capacity, duplicate registration, required fields, account status and entry-fee requirements server-side.

# 7. AI IMPLEMENTATION DIRECTIVES

## P0

Core competition services must not contain game-specific settlement assumptions.

## P1

Implement state transitions as explicit validated operations, not arbitrary status updates.
