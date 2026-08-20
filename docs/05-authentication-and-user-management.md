# AUTHENTICATION AND USER MANAGEMENT

# 1. AUTHENTICATION — P0

Use Supabase Auth with email-based authentication. Supported flows should include registration, login, logout, email verification as configured, password reset and session management.

Phone numbers are collected only as profile/contact/payment information and are not authentication factors.

# 2. PROFILE — P1

Create an application profile after account creation. Reference the Supabase Auth user ID. Store first/last/display name, optional phone data, avatar initial/background key, status and timestamps. Never store passwords in application tables.

# 3. ROLES — P0

At minimum: PLAYER and ADMIN. Role assignment is server-protected and cannot be changed by players.

# 4. AUTHORIZATION — P0

Authentication proves identity; authorization determines capability. Every admin operation requires server-side role verification. RLS must prevent cross-user data access.

# 5. ACCOUNT CHANGES — P1

Email changes and sensitive authentication changes use secure provider mechanisms. Phone changes are validated and audited.

# 6. PRIVACY — P0

Return only minimum necessary data. Never expose private withdrawal destinations or administrative information to other players.

# 7. AI IMPLEMENTATION DIRECTIVES

## P0

Never implement phone authentication, client-controlled roles, client-trusted sessions for authorization, or plaintext passwords.

## P1

Implement protected routes plus server-side authorization and RLS. Test suspended/closed accounts.
