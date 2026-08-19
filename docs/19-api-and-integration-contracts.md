# API AND INTEGRATION CONTRACTS

## General API rules

APIs must use authenticated sessions where required, validate input, return predictable error structures, and avoid exposing internal secrets or unnecessary database columns.

Use stable resource identifiers and explicit status values.

## Domain endpoints

The implementation may expose endpoints/actions for:

- profile/account;
- competitions;
- registration;
- wallet/transactions;
- deposits;
- exchange-rate processing;
- withdrawals;
- notifications;
- support;
- admin operations;
- settlement.

Exact framework-specific routing is an implementation choice unless otherwise specified by the application codebase.

## Payment-provider adapter

Each provider adapter should expose normalized operations such as create payment, query payment status and validate webhook/event. Provider-specific payloads remain inside the adapter.

## Exchange-rate adapter

Expose a normalized operation returning source asset, USD rate, source/provider, timestamp and validity metadata.

## Payout boundary

BG Arena exports a normalized payout instruction rather than calling payout providers. The private payout app handles provider-specific execution.

## Error contract

Errors should distinguish validation, authentication, authorization, not-found, conflict/idempotency, provider-pending, provider-failure and internal errors. Never expose stack traces or secrets to users.

## Webhooks

Webhook handlers must be fast, authenticated, idempotent and resilient to retries. If processing is asynchronous, acknowledge only according to the provider's delivery semantics and persist enough information to process safely.
