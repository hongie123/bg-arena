# BG ARENA — AI SCOPE, FILE OWNERSHIP AND MODIFICATION RULES

# 1. PURPOSE — P0
This document prevents AI coding agents from making unrelated changes or silently crossing domain boundaries.

# 2. OWNERSHIP MODEL — P0
`app/` owns routing/page composition. `features/` owns feature workflows/components. `shared/` owns genuinely reusable UI/types/utilities. `infrastructure/` owns external systems. `financial/` owns money domain operations. `database/` owns migrations/types/seeds. `tests/` owns automated verification.

# 3. FEATURE BOUNDARY — P0
A feature may call the domain/infrastructure services it is authorized to use but must not duplicate their logic. Deposit UI cannot calculate authoritative ledger credit. Competition UI cannot directly debit wallet. Admin UI cannot bypass settlement service.

# 4. SMALLEST SAFE CHANGE — P0
When asked to change one component, modify only necessary files. Do not reformat unrelated files, upgrade dependencies, redesign the app or rewrite architecture unless required and documented.

# 5. FINANCIAL CODE PROTECTION — P0
Changes to ledger, balance, reservations, deposits, settlement or withdrawals require reading all relevant finance/security contracts and adding regression tests. UI-only tasks do not authorize financial changes.

# 6. PROVIDER CODE — P0
NOWPayments/CamPay implementation details remain under `infrastructure/payments`. Do not scatter provider API calls throughout pages or generic services.

# 7. DATABASE CHANGES — P0
Schema changes require migration, updated schema documentation, RLS review and tests. Never modify production schema by undocumented manual SQL.

# 8. DOCUMENTATION SYNC — P1
If code changes a contract, update the appropriate MD file in the same workstream. If the requested code conflicts with architecture, stop and report the conflict.

# 9. AI SELF-CHECK — P0
Before finalizing: Did I cross a boundary? Did I expose a secret? Did I trust client data? Did I alter financial behavior unintentionally? Did I add game-specific core logic? Did I weaken RLS? Did I create duplicate state logic? Did I add tests?

# 10. COMMIT CHECKPOINT — P1
Commits should describe the completed domain change and should not mix unrelated features. The repository should remain buildable after each meaningful checkpoint.
