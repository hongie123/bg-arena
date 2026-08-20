# WITHDRAWALS AND EXTERNAL PAYOUTS

# 1. SEPARATION PRINCIPLE — P0

BG Arena manages the player's financial obligation but does not execute payout-provider API calls.

# 2. REQUEST FLOW — P0

`player -> validation -> funds reservation -> PENDING -> admin notification -> payout instruction -> private payout application -> external payout -> reconciliation -> final status`

# 3. VALIDATION — P0

Validate authenticated account, account status, amount > 0, sufficient available balance, supported method, destination data, eligibility and conflicting withdrawal state before reservation.

# 4. RESERVATION — P0

Reservation is atomic. Reserved funds cannot satisfy another withdrawal or entry fee.

# 5. PAYOUT JSON — P1

Instruction contains withdrawal ID, internal beneficiary reference, payout method, required destination, USD amount, required external conversion information, timestamp and integrity/reference information. Never include BG Arena secrets.

# 6. PRIVATE PAYOUT APPLICATION — P0

The independent application owns its database, authentication, credentials, provider integrations and execution logs. It is a separate trust boundary.

# 7. COMPLETION — P0

External payout outcomes must reference the withdrawal ID. Completion is idempotent.

# 8. FAILURE/AMBIGUITY — P0

Known failure can release reservation according to the state machine. Ambiguous provider state must not automatically release funds until reconciliation.

# 9. PRIVACY — P0

Payout destination data is sensitive and minimized/restricted.

# 10. AI IMPLEMENTATION DIRECTIVES

## P0

Never import payout-provider credentials into BG Arena. Never implement a hidden direct payout call as a shortcut.

## P1

Generate a stable, versioned payout instruction schema that can be manually transferred to the private payout application.
