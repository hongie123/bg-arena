# BG ARENA — SECURITY AND ROW LEVEL SECURITY

# 1. SECURITY OBJECTIVE — P0
Assume the browser is hostile. The server/database must enforce ownership, authorization, financial integrity and secret protection.

# 2. RLS — P0
Every user-owned table must have explicit RLS policies. Default-deny is preferred. Policies should use authenticated identity and protected role/ownership relationships.

# 3. PLAYER ACCESS — P0
Player can read own profile, registrations, deposits, wallet views, transactions, withdrawals, notifications, support and disputes. Player cannot write ledger entries, financial transaction completion, deposit status, exchange rates or settlement execution.

# 4. ADMIN ACCESS — P0
Admin permissions are granular. “Admin” is not a universal permission if finer controls are needed. Sensitive financial operations require explicit permission and server validation.

# 5. SECRETS — P0
Never expose Supabase service-role key, NOWPayments credentials, CamPay credentials, exchange-rate secrets, email provider secrets or payout credentials to browser code. Payout credentials must not exist in BG Arena at all.

# 6. WEBHOOK SECURITY — P0
Verify provider authentication/signatures according to current provider contract. Reject unauthenticated callbacks before any database mutation.

# 7. INPUT SECURITY — P0
Validate all external JSON, form values, IDs, amounts, enums and dynamic fields. Use typed schemas. Sanitize display output and avoid unsafe HTML rendering.

# 8. RATE LIMITING — P1
Apply rate limits to authentication-adjacent endpoints, payment creation, withdrawal requests, support abuse and webhook endpoints where appropriate.

# 9. AUDIT — P0
Privileged/security-sensitive actions are auditable. Logs must avoid secrets and excessive personal data.

# 10. SECURITY TESTS — P0
Cross-user reads, forged role flags, client balance manipulation, forged payment status, replayed webhooks, malformed JSON, IDOR, secret exposure and RLS bypass attempts are mandatory test categories.
