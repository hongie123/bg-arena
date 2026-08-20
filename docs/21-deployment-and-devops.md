# BG ARENA — DEPLOYMENT AND DEVOPS

# 1. ENVIRONMENTS — P0
Use separate development, staging and production configuration. Never copy production secrets into development files or Git.

# 2. ENVIRONMENT VARIABLES — P0
Public browser variables may include Supabase URL/anon key. Server-only variables include service-role key, NOWPayments secrets, CamPay credentials, exchange-rate secrets and email secrets. Payout secrets do not belong in BG Arena.

# 3. GIT — P0
Work in small coherent commits. Do not commit `.env` or secrets. Review diff before push. Migration and code changes that depend on each other should be checkpointed coherently.

# 4. DATABASE DEPLOYMENT — P0
Apply migrations in order. Verify schema and RLS after deployment. Never rely on a manual production dashboard edit as the permanent schema definition.

# 5. BUILD PIPELINE — P1
CI should run install, typecheck, lint, unit/integration/security tests and production build. Fail on TypeScript errors and required test failures.

# 6. WEBHOOK DEPLOYMENT — P0
Verify production callback URLs, secrets/signatures and idempotency before enabling real payment methods. Test provider callbacks against staging/sandbox when available.

# 7. OBSERVABILITY — P1
Monitor errors, latency, payment webhook failures, pending deposits, conversion failures, ledger exceptions and reconciliation discrepancies. Avoid sensitive payload logging.

# 8. BACKUP/RECOVERY — P0
Database backups and recovery procedures must exist. Financial recovery must preserve ledger history and provider references.

# 9. EMERGENCY CONTROLS — P0
System controls must permit disabling new deposits/payment methods during an incident without deleting or corrupting existing records.

# 10. RELEASE CHECKLIST — P0
Migrations applied; RLS verified; secrets configured; payment webhooks verified; financial tests passed; smoke tests passed; rollback/recovery understood; Git commit/tag recorded.
