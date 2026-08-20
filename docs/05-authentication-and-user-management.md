# BG ARENA — AUTHENTICATION AND USER MANAGEMENT

# 1. IDENTITY AUTHORITY — P0
Supabase Auth owns authentication identity. The application profile is an extension of authenticated identity, not a replacement for it.

# 2. AUTH METHODS — P0
Support Google OAuth and email identity according to configured Supabase Auth providers. Phone authentication is explicitly disabled. Do not create password/phone flows outside the approved identity model.

# 3. AUTH CALLBACK — P0
OAuth/email callback validates the authenticated session, creates or updates the profile safely, then redirects according to account state. Never trust redirect parameters to grant authorization.

# 4. PROFILE CREATION — P0
On first authenticated access, create a profile with safe defaults. Username uniqueness must be enforced server-side/database-side. Email comes from trusted auth identity, not a browser-submitted replacement.

# 5. PHONE NUMBER — P1
Collect phone number as profile/payment/support information. Verification, if implemented, verifies contact ownership but does not turn the phone number into a login credential. Do not use phone number equality as payment ownership proof.

# 6. ACCOUNT STATES — P0
Define ACTIVE, RESTRICTED, SUSPENDED and CLOSED semantics. Suspended/restricted accounts must be blocked from appropriate operations while preserving financial history.

# 7. AUTHORIZATION — P0
Authentication answers “who are you?” Authorization answers “what may you do?” Every protected server operation performs authorization independently. Never rely on hidden UI controls as authorization.

# 8. ADMIN IDENTITY — P0
Admin status comes from protected database role/permission records. Never trust a client-provided `is_admin` flag. Privileged actions create audit records.

# 9. SESSION SECURITY — P0
Use secure cookies/session mechanisms supplied by Supabase/Next.js. Do not expose access/refresh tokens in logs. Logout clears local authenticated state and invalidates access according to provider semantics.

# 10. TESTS — P0
Test unauthenticated access, cross-user access, suspended users, admin/non-admin separation, OAuth callback, email identity, profile creation races and attempts to forge authorization fields.
