# SECURITY AND PERMISSIONS

# 1. SECURITY PRINCIPLE — P0

Assume the client is hostile. Validate authorization and business/financial rules server-side.

# 2. SUPABASE SECURITY — P0

Enable RLS for user-facing tables. Follow least privilege. Service-role credentials remain server-side.

# 3. ROLE PROTECTION — P0

Players cannot update role fields. Admin routes perform server-side role checks.

# 4. FINANCIAL SECURITY — P0

Never trust client-supplied balance, exchange rate, payout completion status or settlement result.

# 5. WEBHOOKS — P0

Verify signatures/authentication where supported, reject malformed payloads, record provider event IDs and enforce idempotency.

# 6. SECRETS — P0

Secrets belong in deployment secret storage. Never commit `.env`, API keys, private keys, webhook secrets, payout credentials or service-role keys.

# 7. AUDIT — P0

Record privileged actions with actor, action, target, timestamp, reason and safe before/after snapshots.

# 8. RATE LIMITING — P1

Apply limits to authentication-sensitive endpoints, deposits, withdrawals, support abuse vectors and other high-risk operations.

# 9. INPUT VALIDATION — P0

Validate identifiers, strings, numbers, JSON, uploads, URLs and dynamic fields. Sanitize untrusted HTML.

# 10. DATA MINIMIZATION — P1

Store only necessary personal/payment data and restrict visibility by role/ownership.

# 11. AI IMPLEMENTATION DIRECTIVES

## P0

No frontend-only security controls are sufficient. Test authorization with hostile clients.
