# BG ARENA — SETTLEMENT AND RESULT PROCESSING

# 1. PURPOSE — P0
Settlement converts validated competition results into controlled USD financial credits. It is the bridge between external/game-specific result evidence and the generic financial ledger.

# 2. GAME-AGNOSTIC CORE — P0
BG Arena core does not know PUBG kill rules, CODM scoring, Free Fire scoring or other game-specific formulas. Result data is normalized into a structured contract and interpreted using competition-specific settlement configuration/adapters.

# 3. INPUT TRUST — P0
AI output, imported JSON and administrator-entered result data are untrusted until validated. Never let an AI call directly credit money.

# 4. RESULT IMPORT — P1
Accept structured JSON containing competition reference, extraction metadata, participant/account references and game-specific result metrics. Preserve source/extraction metadata for audit.

# 5. VALIDATION — P0
Validate JSON syntax, schema version, competition identity, registration identity, duplicate participants, numeric ranges, required metrics, settlement-rule compatibility and calculated amounts. Reject unknown users/registrations.

# 6. ADMIN PREVIEW — P0
Before financial mutation, show participants, detected metrics, rule interpretation, proposed USD awards, affected registrations and warnings. Require explicit confirmation.

# 7. ATOMIC EXECUTION — P0
Create settlement header/items and all financial transactions/ledger credits in one transaction. If one critical operation fails, rollback the settlement. Mark settlement completed only after all required credits succeed.

# 8. DUPLICATE PROTECTION — P0
One competition settlement execution is unique by competition/settlement version or equivalent policy key. Replayed imports must not credit twice.

# 9. CORRECTIONS — P0
Corrections after settlement use compensating transactions and a new audited settlement adjustment. Never edit old ledger facts silently.

# 10. TESTS — P0
Test malformed JSON, wrong competition, unknown registration, duplicate player, negative metrics, over-award, duplicate settlement, partial failure rollback and correction flow.
