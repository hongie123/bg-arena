# REGISTRATION SYSTEM

# 1. REGISTRATION FLOW — P0

`authenticate -> load competition -> verify accepting registrations -> verify account -> load dynamic fields -> validate -> check capacity/duplicates -> determine fee -> debit/collect -> create registration -> notify`

The registration and entry-fee relationship must be atomic/coherent.

# 2. DYNAMIC FIELDS — P1

Competition-defined fields may include in-game username, player ID, team, region or other game-specific data. Definitions contain type, label, required flag, order and validation configuration.

# 3. PAYMENT — P0

A failed wallet debit must not create a paid registration. If a deposit is needed first, registration remains unpaid/pending until the USD balance is actually credited.

# 4. CANCELLATION/REFUND — P0

Competition cancellation follows configured refund policy. Refunds are compensating USD ledger credits referencing the original entry transaction and reason.

# 5. MULTIPLE ENTRIES — P1

Default is one registration per player per competition. Multiple entries require explicit competition configuration and unique entry identity.

# 6. AI IMPLEMENTATION DIRECTIVES

## P0

Never trust client-submitted competition price, eligibility or registration status. Recalculate/validate on the server.

## P1

Validate dynamic fields against server-stored definitions and test capacity/duplicate race conditions.
