# BG ARENA — GAMES, COMPETITIONS AND TOURNAMENTS

# 1. CORE RULE — P0
The competition system is game-agnostic. The core must not contain PUBG/CODM/Free Fire scoring formulas, kill logic or game-specific settlement assumptions.

# 2. COMPETITION MODEL — P1
A competition contains identity, game label/reference, format, title, description, rules, schedule, registration window, capacity, entry fee USD, prize configuration, status, custom registration schema and settlement configuration.

# 3. GAME REFERENCE — P1
Game is informational/configurable. A game catalog may be introduced, but adding a game must not require new profile columns or changes to wallet/ledger code.

# 4. LIFECYCLE — P0
DRAFT → PUBLISHED → REGISTRATION_OPEN → REGISTRATION_CLOSED → LIVE → RESULT_PENDING → SETTLEMENT_PENDING → COMPLETED, with CANCELLED/ARCHIVED terminal or policy-specific branches.

# 5. CAPACITY — P1
Registration must enforce capacity atomically to prevent overbooking under concurrent requests.

# 6. ENTRY FEE — P0
Entry fee is USD. Registration must atomically verify available funds, create the financial transaction/ledger debit and registration confirmation. If any step fails, no partial registration/payment remains.

# 7. PRIZES — P1
Prize amounts are USD and must be validated against competition configuration. Settlement may distribute prizes according to external result data and configured rules.

# 8. EDITING — P0
Do not silently modify financially significant published values. Use explicit state/policy for changes to entry fee, prize pool, schedule or capacity.

# 9. DISCOVERY — P1
Player listing supports filtering by game, status, date and format. Never expose unpublished/admin-only competitions.

# 10. TESTS — P0
Test concurrent registration, duplicate registration, insufficient balance, cancelled competition, capacity boundary, schedule boundaries, entry fee atomicity and game-agnostic data storage.
