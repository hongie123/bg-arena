# BG ARENA

# 1. PURPOSE — P0

BG Arena is a game-agnostic competitive gaming platform. This repository contains the implementation contract an AI coding agent can use to build the application.

# 2. START HERE — P0

**Read `CLAUDE.md` first.**

Then read:

1. `docs/00-AI-DEVELOPMENT-MASTER-MANUAL.md`
2. `docs/01-master-platform-specification.md`
3. `docs/02-product-requirements.md`
4. `docs/03-system-architecture.md`
5. `docs/04-supabase-database-schema.md`
6. the specialized domain document required for the feature.

Do not start coding after reading only README. The documentation set is deliberately split into product, architecture, database, security, financial and feature contracts.

# 3. PRIORITY SYSTEM — P0

- `#` = document authority.
- `##` = major implementation area.
- `###` = subsystem/detail.
- `P0` = non-negotiable.
- `P1` = required product behavior.
- `P2` = preferred implementation.
- `P3` = future/optional.

# 4. CORE DECISIONS — P0

- Supabase is the planned backend/database platform.
- Internal accounting and wallet currency is USD.
- Deposit currencies/assets are converted to USD at processing time using a configured exchange-rate source.
- Cameroon launch payment methods include MTN Mobile Money and Orange Money.
- MTN/Orange are explicitly Cameroon-only payment methods.
- Crypto deposits are supported through a provider adapter.
- Authentication is email-based; phone numbers are profile/contact data, not the authentication factor.
- Competitions are game-agnostic and can define their own game-specific registration fields and settlement configuration.
- BG Arena records and reserves withdrawal funds but does not execute payout-provider calls.
- A separate private payout application executes external payouts.
- Financial history is immutable and ledger-backed.

# 5. AI BUILD RULE — P0

The Markdown files are intended to be consumed by an AI coding agent. The agent must treat them as implementation instructions, not as a prompt to invent missing product behavior.

When a requirement is unspecified, choose the smallest safe implementation and document the assumption. When a requirement conflicts with an explicit P0 rule, stop and resolve the conflict instead of silently changing the architecture.
