# BG ARENA — SUPPORT AND DISPUTES

# 1. PURPOSE — P1
Support provides controlled communication and dispute intake without allowing support tools to bypass financial safeguards.

# 2. SUPPORT MODEL — P1
Conversations belong to a player and may reference a competition, deposit, withdrawal or transaction. Messages contain text, sender, timestamps and status.

# 3. DISPUTE MODEL — P0
A dispute references a concrete business object whenever possible. Statuses should include OPEN, UNDER_REVIEW, WAITING_FOR_PLAYER, RESOLVED and CLOSED.

# 4. FINANCIAL DISPUTES — P0
Support agents may investigate and gather evidence. They cannot directly edit balances or ledger records. Financial correction must use approved financial services and audit controls.

# 5. EVIDENCE — P1
Preserve references to deposit/provider/ledger/withdrawal IDs. Avoid storing unnecessary provider payloads or secrets. Sensitive evidence access is permission-controlled.

# 6. ADMIN RESPONSE — P1
Responses should be attributable to an admin/support actor. Internal notes must be distinct from player-visible messages.

# 7. ESCALATION — P1
Disputes involving payment mismatch, duplicate credit, payout uncertainty or ledger discrepancy escalate to financial/admin roles.

# 8. TESTS — P0
Test cross-user conversation access, unauthorized financial adjustment attempts, dispute state transitions and evidence visibility.
