# BG ARENA — SYSTEM ARCHITECTURE

# 1. ARCHITECTURAL GOAL — P0
Build a modular Next.js/TypeScript application with Supabase as the persistence/auth boundary and clear separation between UI, domain services, financial services and external provider adapters.

# 2. LAYERS — P0
## 2.1 Presentation
Next.js routes, server/client components, feature components, forms and view models. Presentation never owns authoritative financial state.
## 2.2 Application/domain
Use-case services coordinate validation, authorization, state transitions and database transactions.
## 2.3 Financial domain
Ledger, balance, reservation, transaction and reconciliation services are isolated and reusable.
## 2.4 Infrastructure
Supabase clients, payment providers, exchange-rate providers, email, logging and configuration.
## 2.5 Database
PostgreSQL schema, constraints, functions where justified, RLS and migrations.

# 3. SERVER/CLIENT BOUNDARY — P0
Browser code may use public Supabase configuration and authenticated user context. Server-only code may use service-role/provider secrets. Never import private modules into client bundles.

# 4. PROVIDER ADAPTERS — P0
`PaymentProvider` defines create/status/verify/webhook capabilities. `NowPaymentsProvider` and `CamPayProvider` implement it. Provider-specific response formats must be normalized before reaching financial services.

# 5. FINANCIAL BOUNDARY — P0
Only protected server-side financial services may create financial transactions, ledger entries, reservations or final deposit credits. A provider adapter reports an external event; it does not decide wallet credit independently.

# 6. STATE MACHINES — P0
Centralize state transition definitions. Reject invalid transitions. Store timestamps for significant transitions. Do not let individual UI components infer business state.

# 7. ERROR MODEL — P1
Errors must be typed by domain: validation, authentication, authorization, provider unavailable, provider mismatch, duplicate, conversion unavailable, financial conflict and unexpected internal error. User-facing messages must not reveal secrets or internal stack traces.

# 8. OBSERVABILITY — P1
Log correlation IDs, domain references and provider references without logging secrets. Financial logs must allow an investigator to trace a deposit without exposing credentials or unnecessary personal data.

# 9. EXTENSIBILITY — P1
Adding a provider, game, payment network or competition format should add an adapter/configuration/domain module rather than alter unrelated wallet/identity code.
