# BG ARENA — SUPABASE DATABASE SCHEMA

This document defines the logical schema. Exact SQL migrations must preserve these relationships and financial invariants.

## 1. profiles

Purpose: application profile linked to Supabase Auth.

Suggested columns:

- `id` UUID primary key, references `auth.users.id`;
- `email` or auth-derived email reference as appropriate;
- `display_name` text;
- `first_name` text;
- `last_name` text;
- `phone_number` text nullable;
- `phone_country_code` text nullable;
- `avatar_initial` text;
- `avatar_background_key` text;
- `role` enum such as PLAYER/ADMIN;
- `status` enum such as ACTIVE/SUSPENDED/CLOSED;
- `created_at` timestamptz;
- `updated_at` timestamptz.

Do not store game-specific IDs here.

## 2. wallets

One primary USD wallet per player.

Columns:

- `id` UUID primary key;
- `user_id` UUID foreign key to profiles;
- `currency` fixed to USD;
- `available_minor` bigint;
- `reserved_minor` bigint;
- `status` enum;
- timestamps.

A unique constraint must enforce one primary wallet per user.

## 3. wallet_transactions

Immutable financial ledger.

Columns should include:

- `id` UUID;
- `wallet_id` UUID;
- `user_id` UUID;
- `transaction_type` enum;
- `direction` CREDIT/DEBIT;
- `amount_minor` bigint positive;
- `currency` USD;
- `balance_before_minor` bigint where useful;
- `balance_after_minor` bigint where useful;
- `reference_type`;
- `reference_id`;
- `idempotency_key` unique when applicable;
- `description`;
- `metadata` JSONB;
- `created_at`.

Never edit a historical transaction to correct it. Use a compensating transaction.

## 4. competitions

Columns:

- `id` UUID;
- `game_id` or game identifier;
- `title`;
- `description`;
- `format`;
- `status`;
- `registration_opens_at`;
- `registration_closes_at`;
- `starts_at`;
- `ends_at` nullable;
- `capacity` nullable;
- `entry_fee_minor_usd`;
- `prize_pool_minor_usd` or computed prize configuration;
- `rules` JSONB/text;
- `settlement_config` JSONB or normalized rule references;
- timestamps.

## 5. games

Optional normalized game catalog.

Columns:

- `id`;
- `name`;
- `slug` unique;
- `status`;
- `description`;
- timestamps.

Game records describe games; they must not force game-specific columns into profiles.

## 6. competition_registration_fields

Defines dynamic fields required for a competition.

Columns:

- `id`;
- `competition_id`;
- `field_key`;
- `label`;
- `field_type`;
- `required`;
- `validation_config` JSONB;
- `display_order`;
- timestamps.

Examples include player ID, in-game name, team name, region or other game-specific values.

## 7. registrations

Columns:

- `id`;
- `competition_id`;
- `user_id`;
- `status`;
- `registered_at`;
- `entry_fee_transaction_id` nullable;
- timestamps.

Use a unique constraint appropriate to the competition's multiple-entry policy.

## 8. registration_field_values

Columns:

- `id`;
- `registration_id`;
- `field_id`;
- `value_text` or typed value representation;
- timestamps.

## 9. deposits

Columns should preserve the complete conversion/payment lifecycle:

- `id`;
- `user_id`;
- `wallet_id`;
- `provider`;
- `provider_payment_id`;
- `provider_event_id`;
- `source_type` FIAT/CRYPTO;
- `source_currency_or_asset`;
- `source_amount` exact decimal representation;
- `exchange_rate` exact decimal representation;
- `rate_source`;
- `rate_timestamp`;
- `usd_amount_minor`;
- `provider_fee` if applicable;
- `status`;
- `raw_provider_reference`/safe metadata JSONB;
- `credited_transaction_id`;
- timestamps.

Unique constraints must prevent duplicate provider events and duplicate credits.

## 10. withdrawals

Columns:

- `id`;
- `user_id`;
- `wallet_id`;
- `amount_minor_usd`;
- `payout_method`;
- `destination_reference` stored securely/minimized;
- `status`;
- `reserved_transaction_id`;
- `payout_instruction_version`;
- `external_payout_reference` nullable;
- `failure_reason` nullable;
- `requested_at`;
- `processed_at`;
- timestamps.

Do not store payout-provider secret credentials here.

## 11. settlement_runs

Columns:

- `id`;
- `competition_id`;
- `status`;
- `source_type`;
- `source_reference`;
- `imported_result` JSONB;
- `validated_result` JSONB;
- `calculation_snapshot` JSONB;
- `confirmed_by`;
- `confirmed_at`;
- `completed_at`;
- timestamps.

Unique business constraints must prevent a completed competition from being financially settled twice.

## 12. settlement_results

Normalized per-participant settlement outputs:

- `id`;
- `settlement_run_id`;
- `registration_id`;
- `rank` nullable;
- game-specific metrics in controlled JSONB/configurable fields;
- `winnings_minor_usd`;
- `wallet_transaction_id`;
- timestamps.

## 13. notifications

Columns:

- `id`;
- `user_id` nullable for global/admin notices;
- `type`;
- `title`;
- `body`;
- `data` JSONB;
- `read_at`;
- `created_at`.

## 14. support_tickets

Columns:

- `id`;
- `user_id`;
- `subject`;
- `status`;
- `priority`;
- `category`;
- `assigned_admin_id`;
- timestamps.

## 15. support_messages

Columns:

- `id`;
- `ticket_id`;
- `sender_user_id`;
- `body`;
- `created_at`.

## 16. audit_logs

Columns:

- `id`;
- `actor_user_id`;
- `action`;
- `entity_type`;
- `entity_id`;
- `before_data` JSONB nullable;
- `after_data` JSONB nullable;
- `reason`;
- `request_id`;
- `created_at`.

## 17. exchange_rate_snapshots

Optional normalized table for auditable rate history:

- `id`;
- `base_currency_or_asset`;
- `quote_currency` fixed USD;
- `rate` exact numeric;
- `provider`;
- `fetched_at`;
- `raw_reference` safe metadata.

## 18. Row Level Security

Players may read/update only permitted profile fields and read their own wallets, transactions, registrations, deposits, withdrawals, results, notifications and support tickets.

Players must never update:

- wallet balances;
- ledger transactions;
- settlement results;
- payment statuses;
- withdrawal approval states;
- admin roles.

Administrators receive narrowly scoped policies appropriate to their role.

Privileged financial mutations should use controlled server-side functions rather than broad client write permissions.

## 19. Indexes

Index foreign keys and common operational queries, including:

- profiles by email/status;
- competitions by status/start date;
- registrations by competition/user;
- wallet transactions by wallet/date;
- deposits by provider reference/status;
- withdrawals by status/user/date;
- settlement runs by competition/status;
- notifications by user/read state;
- audit logs by entity/date.

## 20. Migration rules

All schema changes must be versioned as migrations. Never manually alter production schema without recording the migration.
