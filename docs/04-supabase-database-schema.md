# BG ARENA — SUPABASE DATABASE SCHEMA

# 1. DATABASE AUTHORITY — P0
Supabase PostgreSQL is the authoritative application database. Migrations define schema. Production schema must never be changed manually without a migration/checkpoint.

# 2. IDENTITY TABLES — P0
`profiles`: id, user_id, first_name, last_name, username, email, phone_number, phone_verified, country_code, account_status, created_at, updated_at. `admin_roles`: user_id, role, status, created_at, updated_at. Auth identity remains in Supabase Auth.

# 3. COMPETITION TABLES — P1
`competitions`, `competition_custom_fields`, `competition_custom_field_options`, `competition_registrations`, `competition_registration_values`. Foreign keys must enforce ownership relationships. Dynamic fields allow game-specific identifiers without polluting profiles.

# 4. FINANCE TABLES — P0
`wallet_accounts`, `ledger_entries`, `financial_transactions`, `financial_reservations`, `deposits`, `payment_methods`, `payment_provider_transactions`, `payment_events`, `exchange_rate_snapshots`.

# 5. SETTLEMENT/WITHDRAWAL TABLES — P0
`settlements`, `settlement_items`, `withdrawals`, `withdrawal_events`.

# 6. COMMUNICATION TABLES — P1
`notifications`, `support_conversations`, `support_messages`, `disputes`.

# 7. ADMIN TABLES — P0
`audit_logs`, `system_controls`.

# 8. FINANCIAL COLUMN RULES — P0
Internal money must be represented using integer minor units or exact numeric/decimal, never binary floating point. Store currency explicitly even though wallet currency is USD. Provider amounts retain source currency/asset. Use UTC timestamps.

# 9. REQUIRED CONSTRAINTS — P0
Unique deposit reference; unique idempotency key; unique provider+provider transaction ID where provider guarantees it; valid foreign keys; non-negative amounts where applicable; valid status values; one primary USD wallet per player; no orphan financial records.

# 10. LEDGER IMMUTABILITY — P0
Ledger rows are append-only. Corrections use compensating transactions. RLS must prevent normal players from inserting/updating/deleting ledger rows.

# 11. RLS MODEL — P0
Player SELECT policies use `auth.uid()` against owner/user ID. Financial INSERT/UPDATE is server-side only. Admin SELECT/write policies require explicit role/permission checks. Service-role access is never exposed to browser clients.

# 12. MIGRATION ORDER — P0
Foundation → profiles → competitions → wallet → ledger → transactions → reservations → payment methods → deposits → provider transactions → payment events → rate snapshots → settlements → withdrawals → notifications/support/disputes → audit/system controls.

# 13. MIGRATION QUALITY — P1
Every migration must be deterministic, reviewable and reversible where practical. Seed only non-secret configuration. Never put provider credentials in seed data.
