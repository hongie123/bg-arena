# BG ARENA

# 1. PURPOSE — P0
BG Arena is a game-agnostic competitive gaming platform with a USD-denominated internal wallet, competition registration, controlled settlement, deposits through crypto and Cameroon mobile money, and manually controlled external payouts.

# 2. THIS REPOSITORY — P0
This repository is the development specification and eventual application codebase. The `docs/` directory is intentionally detailed enough for Claude, Antigravity or another coding agent to implement the application without requiring a second giant conversational prompt.

# 3. START HERE — P0
Read in this order:
1. `CLAUDE.md`.
2. `docs/00-AI-DEVELOPMENT-MASTER-MANUAL.md`.
3. `docs/01-master-platform-specification.md`.
4. `docs/24-technical-architecture-and-development-blueprint.md`.
5. The domain document for the requested feature.

# 4. ARCHITECTURE SUMMARY — P0
Frontend: Next.js + React + TypeScript.
Backend: Next.js server-side application/services + Supabase.
Database: Supabase PostgreSQL.
Auth: Supabase Auth with Google/email identity.
Crypto deposits: NOWPayments adapter.
Mobile money deposits: CamPay adapter, MTN Cameroon and Orange Cameroon.
Internal accounting: USD-only immutable ledger.
Payouts: external private payout application; BG Arena exports instructions and reconciles outcomes.

# 5. ENGINEERING PRINCIPLE — P0
Architecture first. Scope second. Implementation third. Testing fourth. Git checkpoint fifth.

# 6. DOCUMENT MAP — P1
`00` AI operating manual; `01` master platform; `02` product requirements; `03` system architecture; `04` database; `05` auth/users; `06` player dashboard; `07` admin; `08` competitions; `09` registration; `10` wallet/ledger; `11` payments/deposits; `12` exchange rates; `13` withdrawals; `14` settlement; `15` notifications; `16` support/disputes; `17` security/RLS; `18` API contracts; `19` design system; `20` testing; `21` deployment; `22` reconciliation/audit; `23` AI/file ownership; `24` technical blueprint.
