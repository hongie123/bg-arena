# SETTLEMENT ENGINE

# 1. PURPOSE — P0

Convert validated competition results into deterministic financial outcomes.

# 2. WORKFLOW — P0

`identify competition -> import result -> validate schema -> resolve registrations -> validate metrics -> calculate -> preview -> admin confirmation -> atomic settlement -> winnings credits -> complete -> notify`

# 3. INPUT — P1

Inputs identify competition, source, participants and game-specific metrics such as rank, kills, finishes, score, team or outcome according to competition configuration.

# 4. VALIDATION — P0

Reject unknown competitions, nonexistent registrations, duplicate participants, invalid numeric values, impossible ranks, unauthorized result changes and totals exceeding configured limits.

# 5. DETERMINISM — P0

The same validated input/configuration must produce the same financial output. Persist calculation/configuration snapshots.

# 6. GAME-SPECIFIC RULES — P0

PUBG-style concepts such as knocks, finishes, environmental deaths and revives belong to a PUBG-specific adapter/configuration. They must never become generic assumptions.

# 7. ATOMICITY — P0

Settlement cannot partially credit winners while marking the run complete. Use a transactional boundary.

# 8. DUPLICATE PROTECTION — P0

A completed settlement cannot execute again. Retry returns the existing outcome.

# 9. CORRECTIONS — P0

Confirmed settlement is not silently edited. Corrections use controlled reversal/adjustment, authorization, reason and audit.

# 10. AI IMPLEMENTATION DIRECTIVES

## P0

AI extraction output is untrusted. Only validated settlement code can create wallet transactions.

## P1

Persist imported input, validated output and calculation preview so administrators can audit exactly what was settled.
