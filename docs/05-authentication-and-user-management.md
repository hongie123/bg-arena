# AUTHENTICATION AND USER MANAGEMENT

## Authentication

Use Supabase Auth with email-based authentication. Supported flows should include registration, login, logout, email verification as configured, password reset and session management.

Phone numbers are collected only as profile/contact/payment information and are not authentication factors.

## Profile

Create an application profile after successful account creation. The profile references the Supabase Auth user ID.

Store first/last/display name, optional phone data, avatar initial/background key, status and timestamps. Do not store passwords in application tables.

## Roles

At minimum: PLAYER and ADMIN. Role assignment must be protected from client modification.

A suspended/closed account must be prevented from actions according to status policy. Existing financial history remains auditable.

## Authorization

Authentication proves identity; authorization determines what the user can do. Every admin operation requires server-side role verification.

## Account changes

Email changes and other sensitive changes must use the authentication provider's secure mechanisms. Phone changes should be validated and audited.

## Privacy

Return only the minimum profile/payment data required by each UI. Do not expose private withdrawal destination details to other players.
