# RESULT EXTRACTION AND AI

# 1. PURPOSE — P1

AI may assist administrators by extracting structured competition results from spectator recordings, screenshots, documents or other evidence.

# 2. TRUST BOUNDARY — P0

AI output is untrusted data. It is never a financial authority and cannot directly call wallet-credit operations.

# 3. PIPELINE — P0

`evidence -> extraction model -> structured JSON -> schema validation -> business validation -> admin preview -> settlement engine`

# 4. STRUCTURED OUTPUT — P1

Payload should identify competition, extraction timestamp/source, participant/registration references, normalized metrics, confidence/uncertainty and evidence reference where available.

# 5. VALIDATION — P0

Validate JSON syntax, schema, competition identity, participant mapping, required fields, numeric ranges, duplicates and settlement compatibility. Reject malformed/ambiguous output.

# 6. HUMAN REVIEW — P0

Administrators inspect extracted results before settlement. Uncertain mappings require explicit correction or confirmation.

# 7. GAME-SPECIFIC EXTRACTION — P1

Extraction schemas may differ by game. Normalize validated outputs before feeding the generic settlement engine.

# 8. AI IMPLEMENTATION DIRECTIVES

## P0

Never execute arbitrary code, SQL, financial mutations or privileged actions from model output.

## P1

Persist extraction input/output and validation status so administrators can audit and reproduce decisions.
