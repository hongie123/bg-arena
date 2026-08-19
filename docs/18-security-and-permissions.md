# SECURITY AND PERMISSIONS

## Principles

Assume the client is hostile. Validate authorization and all financial/business rules server-side.

## Supabase security

Enable Row Level Security for user-facing tables. Policies should follow least privilege. Service-role credentials remain server-side only.

## Role protection

Role fields cannot be updated by players. Admin routes must perform server-side role checks.

## Financial security

No client-supplied balance, exchange rate, payout completion status or settlement result can be trusted as authoritative.

## Webhooks

Verify provider signatures/authentication where supported, reject malformed payloads, record provider event IDs and enforce idempotency.

## Secrets

Secrets belong in deployment environment secret storage. Never commit `.env`, API keys, private keys, webhook signing secrets, payout credentials or database service-role keys.

## Audit

Record privileged financial/administrative actions with actor, action, target, timestamp, reason and relevant before/after snapshots where safe.

## Rate limiting

Apply appropriate rate limits to authentication-sensitive endpoints, deposit creation, withdrawal creation, support abuse vectors and other high-risk endpoints.

## Input validation

Validate strings, identifiers, numeric values, JSON payloads, file uploads, URLs and dynamic registration fields. Do not render untrusted HTML without sanitization.

## Data minimization

Store only necessary personal/payment data and restrict visibility according to role and ownership.
