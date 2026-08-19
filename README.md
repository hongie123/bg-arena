# BG ARENA

BG Arena is a game-agnostic competitive gaming platform. This repository contains the implementation specification that an AI coding agent can use to build the application.

## Start here

**Read `CLAUDE.md` first.** It defines the engineering rules and source-of-truth hierarchy.

Then read:

1. `docs/01-master-platform-specification.md`
2. `docs/02-product-requirements.md`
3. `docs/03-system-architecture.md`
4. `docs/04-supabase-database-schema.md`
5. the remaining domain-specific documents relevant to the feature being implemented.

## Core decisions

- Supabase is the planned backend/database platform.
- Internal accounting and wallet currency is USD.
- Deposit currencies/assets are converted to USD at processing time using a configured exchange-rate source.
- Cameroon launch payment methods include MTN Mobile Money and Orange Money.
- Crypto deposits are supported through a provider adapter.
- Authentication is email-based; phone numbers are profile/contact data, not the authentication factor.
- Competitions are game-agnostic at the core and can define their own game-specific registration fields and settlement configuration.
- BG Arena records and reserves withdrawal funds but does not execute payout-provider calls.
- A separate private payout application executes external payouts.
- Financial history is immutable and ledger-backed.

## Documentation structure

The `docs/` directory is the detailed product and engineering contract. Do not treat the files as generic marketing documentation. They define implementation behavior, data boundaries, security rules and financial invariants.
