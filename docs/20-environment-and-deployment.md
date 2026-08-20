# ENVIRONMENT AND DEPLOYMENT

# 1. ENVIRONMENTS — P1

Maintain separate development, staging and production configuration where practical.

# 2. ENVIRONMENT VARIABLES — P0

Inject environment-specific values through deployment configuration. Local `.env` files containing secrets must be ignored by Git.

Typical categories: Supabase URL/public key, server service-role key, payment credentials, webhook secrets, exchange-rate credentials, email credentials, application URL and private payout integration configuration.

Only browser-safe values may enter client bundles.

# 3. DATABASE DEPLOYMENT — P0

Apply migrations in order. Back up production before risky migrations. Never make irreversible schema changes without migration/recovery planning.

# 4. PRODUCTION DEPLOYMENT — P0

Fail builds when required non-secret configuration is missing. Never print secrets in logs.

# 5. HOSTING — P1

If Vercel or another frontend host is used, configure secrets in project environment settings. Server functions receive only required secrets.

# 6. MONITORING — P1

Monitor authentication failures, payment/webhook failures, settlement failures, withdrawal failures and unexpected database errors.

# 7. AI IMPLEMENTATION DIRECTIVES

## P0

Never commit secrets or use production credentials in source code.

## P1

Deployment must be reproducible from repository code plus environment configuration.
