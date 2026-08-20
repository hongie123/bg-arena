# BG ARENA — WITHDRAWALS AND EXTERNAL PAYOUTS

# 1. SECURITY BOUNDARY — P0
BG Arena manages withdrawal requests and financial reservations but does not execute external payouts. Payout credentials and API calls belong exclusively to the private payout application.

# 2. REQUEST FLOW — P0
Player submits amount/destination → server validates → check account restrictions → check available USD → create reservation atomically → create withdrawal → PENDING → notify admin → generate payout instruction JSON.

# 3. RESERVATION — P0
Reserve requested USD before the payout is handed off. Available balance decreases immediately while total ledger ownership remains represented. Reservation states are explicit.

# 4. PAYOUT DATA — P0
Generated payout JSON must contain only the minimum information required by the private payout application: withdrawal reference, player/account reference, payout method, destination, amount/currency as approved and metadata needed for reconciliation. Never include BG Arena secrets.

# 5. EXTERNAL OUTCOME — P0
Possible outcomes: completed, failed, rejected, cancelled, unknown/pending. Unknown external outcome must not automatically release funds. Keep reservation active until reconciliation.

# 6. COMPLETION — P0
On verified external success, consume/finalize reservation and create the corresponding financial transaction. On verified failure/rejection, release reservation according to policy. Every transition is audited.

# 7. ADMIN CONTROL — P0
Admin may approve/reject/reconcile according to permissions. A withdrawal cannot be completed solely by changing a UI status; the server must enforce the valid transition and evidence requirements.

# 8. PLAYER EXPERIENCE — P1
Show requested USD amount, destination summary (masked), status, created date and status history. Never show provider secrets or internal credentials.

# 9. FRAUD/SAFETY — P0
Validate minimum/maximum withdrawal, account status, available funds, destination format and required player verification. Never trust a client-supplied available balance.

# 10. TESTS — P0
Test insufficient funds, concurrent withdrawal requests, duplicate submission, reservation race, admin authorization, external success, external failure, unknown outcome and safe recovery.
