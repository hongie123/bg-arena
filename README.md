# BG ARENA

# 1. PURPOSE — P0

BG Arena is a modular, game-agnostic competitive gaming platform with a USD-denominated internal wallet and controlled external payment, settlement and payout boundaries.

# 2. AUTHORITATIVE DOCUMENTATION — P0

The repository is specification-first. Read in this order:

1. `CLAUDE.md`
2. `docs/00-AI-DEVELOPMENT-MASTER-MANUAL.md`
3. `docs/01-master-platform-specification.md`
4. `docs/24-technical-architecture-and-development-blueprint.md`
5. Domain-specific documents relevant to the task.

# 3. CORE MODEL — P0

- Supabase PostgreSQL is the primary database.
- Supabase Auth provides Google/email identity.
- Phone authentication is not used.
- Internal wallet currency is USD only.
- Crypto deposits use NOWPayments.
- Cameroon Mobile Money deposits use CamPay with MTN Cameroon and Orange Cameroon.
- Payout execution is external and manually controlled through the private payout application.
- The immutable financial ledger is the accounting source of truth.
- Game-specific result logic is outside the BG Arena core.

# 4. DEVELOPMENT ORDER — P1

Foundation → database/RLS → authentication/profiles → wallet/ledger → payment adapters → exchange rates → deposits → competitions/registrations → dashboards → settlements → withdrawals/payout export → reconciliation → administration/support → hardening/deployment.

# 5. AI DEVELOPMENT RULE — P0

Do not ask an AI coding agent to “build the app” while ignoring the repository specifications. The intended workflow is to give the repository to the coding agent, instruct it to read `CLAUDE.md` and the documentation in the required order, then implement the application incrementally while preserving the documented architecture.
