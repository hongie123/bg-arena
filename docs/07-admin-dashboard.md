# ADMIN DASHBOARD

## Overview

Provide operational summaries for users, competitions, registrations, payments, pending withdrawals, pending settlements and reconciliation.

## Users

Search/filter users, inspect profile/status/history and perform permitted account actions. Sensitive operations require confirmation and audit logging.

## Competitions

Create, edit, publish, open/close registration, start, cancel and complete competitions according to valid state transitions. Configure entry fees, prize structures, registration fields and settlement rules.

## Registrations

View participants, registration field values, payment state and registration status. Do not expose secrets or unnecessary private information.

## Settlements

Import structured results, validate them, preview calculations, inspect participant mappings and explicitly confirm settlement. A settlement confirmation is a high-risk financial action and must be audited.

## Payments

View deposits/payment events, provider references, status, conversion details and failures. Admins may reconcile/resolve according to authorized workflows but must not arbitrarily alter immutable ledger history.

## Wallet/Transactions

Search transactions by player/reference/type/date and inspect the underlying source record. Administrative adjustments require a reason and compensating ledger entry.

## Withdrawals

Review pending requests, validate eligibility/history, reserve/release funds according to state, generate payout instructions and record external payout references/outcomes.

## Financial Reconciliation

Calculate player liabilities and group them by payment method/status/date. Show discrepancies and unresolved records.

## Notifications

Create and manage authorized platform announcements and user notifications.

## Support

Assign/respond/close tickets and retain message history.

## Admin safety

Never place provider secret keys in the dashboard. Never rely on hidden frontend routes for authorization. Every privileged action is server-authorized and audited.
