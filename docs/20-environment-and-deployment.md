# ENVIRONMENT AND DEPLOYMENT

## Environments

Maintain separate development, staging and production configuration where practical.

## Environment variables

Secrets and environment-specific values must be injected by the deployment platform. `.env` files are for local development only and must be ignored by Git when they contain secrets.

Typical categories include:

- Supabase URL;
- public Supabase anon/publishable key;
- server-side Supabase service-role key;
- payment-provider credentials;
- webhook signing secrets;
- exchange-rate provider credentials;
- email provider credentials;
- application URL;
- private payout integration configuration.

Only browser-safe values may be exposed to client bundles.

## Database deployment

Apply migrations in order. Back up production before risky migrations. Never make an irreversible production schema change without a migration and rollback/recovery plan.

## Deployment

Production builds must fail if required non-secret configuration is missing. Secret values must not be printed in logs.

## Vercel/hosting principle

If Vercel or another frontend host is used, environment variables must be configured in its project settings rather than committing secrets to the repository. Server-side functions must receive only the secrets they actually require.

## Monitoring

Monitor authentication errors, payment/webhook failures, settlement failures, withdrawal failures and unexpected database errors.
