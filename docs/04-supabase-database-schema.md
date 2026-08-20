# BG ARENA — SUPABASE DATABASE SCHEMA

# 1. SCHEMA AUTHORITY — P0

This document defines the logical schema. SQL migrations must preserve these relationships, constraints and financial invariants.

## profiles — P0

Application profile linked to `auth.users`. Store identity/contact/display data, protected role/status and avatar initial/background key. Never store game-specific IDs or passwords here.

## wallets — P0

One primary USD wallet per player. Store available and reserved minor units. Enforce one primary wallet per user and USD as the only internal currency.

## wallet_transactions — P0

Immutable ledger. Include wallet/user, transaction type, direction, exact amount, currency, source reference, idempotency key, timestamps and safe metadata. Corrections use compensating entries.

## competitions — P1

Store game reference, title, description, format, status, registration/event times, capacity, USD entry fee, prize configuration, rules and settlement configuration.

## games — P1

Normalized game catalog. Game records must not force game-specific fields into profiles.

## competition_registration_fields — P1

Dynamic field definitions with key, label, type, required flag, validation configuration and display order.

## registrations — P0

Player/competition relationship with status, timestamp and entry-fee reference. Enforce duplicate-entry policy at database/business-rule level.

## registration_field_values — P0

Values submitted for the competition-defined fields. Validate according to the field definition.

## deposits — P0

Preserve provider, provider payment/event IDs, source type, source currency/asset, exact source amount, exact exchange rate, rate source/timestamp, USD credit, fees, status and resulting ledger transaction. Unique provider references prevent duplicate credits.

## withdrawals — P0

Store requested USD amount, payout method, minimized destination reference, status, reservation transaction, payout instruction version and external reference. Never store payout-provider credentials.

## settlement_runs / settlement_results — P0

Persist imported/validated result snapshots, calculation snapshot, confirmation actor/time and per-registration winnings. Constraints prevent duplicate financial settlement.

## notifications / support / audit — P1

Use ownership/role-based access. Audit records are append-oriented and must not contain secrets.

## exchange_rate_snapshots — P0

Optionally persist normalized rate snapshots containing source asset, USD quote, exact rate, provider and timestamp. Deposit records must independently preserve the rate actually used.

# 2. RLS — P0

Players can access only permitted rows belonging to themselves. Players cannot update balances, ledger entries, settlement outcomes, payment status, withdrawal approval state or roles. Admin access is narrowly scoped and server-authorized.

# 3. CONSTRAINTS AND INDEXES — P0

Foreign keys, unique provider references, unique idempotency keys, wallet ownership, non-negative monetary values and appropriate status constraints must be enforced. Index operational queries for users, competitions, registrations, transactions, deposits, withdrawals, settlements, notifications and audit logs.

# 4. MIGRATIONS — P0

Every schema change is a versioned migration. Never manually alter production schema outside the migration process.

# 5. AI IMPLEMENTATION DIRECTIVES

## P0

The database is a security boundary, not merely storage. Encode important invariants with PostgreSQL constraints/RLS where practical.

## P1

Create migrations in dependency order and test RLS with both player and admin identities.

## P2

Prefer normalized relationships for core entities and JSONB only where configuration is genuinely dynamic.
