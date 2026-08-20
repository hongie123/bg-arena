# BG ARENA — REGISTRATION SYSTEM

# 1. PURPOSE — P1
Registration connects a player to a specific competition and captures that competition's required information without permanently modifying the player's generic profile.

# 2. DATA MODEL — P0
Registration references user, competition, status, timestamps, payment/financial transaction reference and dynamic field values. Dynamic values reference competition-defined custom fields.

# 3. CUSTOM FIELDS — P1
Fields can represent game ID, username, team, region, device or other competition-specific information. Each field defines type, required flag, validation constraints, display order and optional choices.

# 4. VALIDATION — P0
Server validates every dynamic value against the competition schema. Client validation is only a UX aid. Reject unknown fields, invalid types, missing required values and values outside constraints.

# 5. REGISTRATION FLOW — P0
Check authentication → load published competition → verify registration window → verify capacity → validate fields → check duplicate registration → verify USD funds → atomically debit entry fee and create registration → return authoritative registration state.

# 6. DUPLICATES — P0
A player cannot create two active registrations for the same competition unless the competition explicitly supports multiple slots/entries. Database constraints should enforce the default rule.

# 7. PAYMENT LINK — P0
Registration payment must reference a financial transaction. Never mark a registration paid merely because the client sent `paid=true`.

# 8. CANCELLATION/REFUND — P1
If policy allows cancellation/refund, use a compensating financial transaction and audit event. Never delete the original ledger entry.

# 9. PLAYER VIEW — P1
Show registration status, competition information, submitted values where appropriate, payment status and relevant instructions.

# 10. TESTS — P0
Required tests include duplicate registration, capacity race, insufficient funds, invalid dynamic fields, closed registration, authorization and atomic payment/registration behavior.
