# BG ARENA — MASTER PLATFORM SPECIFICATION

## 1. Product identity

BG Arena is a competitive gaming platform initially designed for the Cameroon market and intended to expand to other markets, beginning with Nigeria and later additional countries.

The platform allows players to discover competitions, register, pay entry fees, participate, receive competition results and winnings, manage a USD-denominated wallet, request withdrawals, and review their financial and competition history.

BG Arena must remain **game-agnostic**. The platform infrastructure must support multiple games without changing the core wallet, user, registration, competition, or financial architecture.

## 2. Product goals

The system must:

- provide a simple player experience;
- support competitive gaming tournaments and matches;
- allow administrators to create and manage competitions;
- collect competition entry fees;
- maintain accurate player balances;
- support deposits using launch-market payment methods and crypto;
- convert deposited value into USD for internal accounting;
- support competition-specific registration information;
- process structured results and settlements safely;
- accept withdrawal requests without directly executing payouts;
- produce auditable financial records;
- provide administrators with reconciliation tools;
- provide a foundation that can scale beyond Cameroon.

## 3. Core architectural decisions

### 3.1 Internal currency

USD is the single internal accounting currency.

All wallet balances, entry fees, prize amounts, winnings, reserves, ledger entries and internal financial calculations are denominated in USD.

The system may accept deposits in XAF, other supported fiat currencies, or supported crypto assets. Those are external deposit denominations only. After a deposit is confirmed, BG Arena calculates the USD equivalent using the configured exchange-rate mechanism and credits the USD wallet.

The original deposit denomination and conversion information must remain stored for auditability.

### 3.2 Authentication

Authentication is by email.

The platform may collect a phone number for contact/profile/payment purposes, but the phone number is not an authentication credential and must not become an implicit login method.

### 3.3 Database

Supabase is the planned primary backend platform, including PostgreSQL database, authentication, Row Level Security, storage where explicitly required, and server-side functions/integration boundaries where appropriate.

### 3.4 Payout separation

BG Arena does not directly execute player payouts.

BG Arena owns:

- withdrawal requests;
- eligibility validation;
- balance reservation;
- withdrawal status;
- financial history;
- payout instruction generation;
- administrator notification;
- reconciliation.

A separate private payout application owns:

- payout-provider credentials;
- payout-provider API calls;
- payout execution;
- provider-specific payout operations.

This separation is a security boundary.

## 4. Launch payment architecture

For the Cameroon launch, supported deposit methods include:

- MTN Mobile Money;
- Orange Money;
- cryptocurrency through the configured crypto-payment provider.

Mobile-money methods must be explicitly marked as Cameroon-only in product configuration/UI. The architecture must allow other country-specific methods to be added later without redesigning the wallet.

The crypto layer must be provider-abstracted so that a provider such as NOWPayments can be integrated without coupling the wallet ledger to one vendor.

## 5. Deposit lifecycle

A deposit must not be credited merely because the client displays a success screen.

The authoritative lifecycle is:

1. player selects a deposit method;
2. player supplies the requested deposit amount and required payment information;
3. BG Arena creates a deposit/payment record;
4. provider-side payment is initiated or instructions are displayed;
5. provider confirmation/webhook or trusted administrative confirmation is received;
6. the event is authenticated and validated;
7. idempotency is checked;
8. the original amount and currency/asset are recorded;
9. the current configured exchange rate is obtained;
10. the USD equivalent is calculated;
11. the wallet ledger receives one credit transaction;
12. the deposit is marked credited;
13. the user balance reflects the ledger-backed credit;
14. an audit event is recorded.

The client must never be able to submit a USD amount and cause that amount to be credited without server-side calculation.

## 6. Exchange-rate requirement

Whenever a deposit is credited, the system must obtain the applicable conversion rate from the configured exchange-rate API/service at processing time.

The deposit record must retain at minimum:

- source currency or crypto asset;
- source amount;
- USD conversion rate;
- USD amount credited;
- rate-provider/source identifier;
- rate timestamp;
- provider/payment reference;
- processing timestamp.

If a reliable rate cannot be obtained, the system must fail safely and must not guess a rate or credit an unverified USD amount.

## 7. Wallet

Each player has one primary USD wallet.

The wallet must expose at least:

- available balance;
- reserved balance;
- total ledger balance where useful;
- currency = USD.

Financial history is represented through an immutable transaction ledger rather than arbitrary balance mutations.

Typical transaction types include:

- deposit credit;
- competition entry fee;
- competition prize/winnings credit;
- withdrawal reservation;
- withdrawal release;
- withdrawal completion adjustment where applicable;
- refund;
- administrative adjustment;
- fee;
- correction/reversal.

Every transaction must have an audit trail and a unique idempotency/reference key where applicable.

## 8. Competition architecture

A competition is a configurable entity that can represent a tournament, match, event, or other competitive format.

A competition may define:

- game;
- title;
- description;
- rules;
- schedule;
- registration open/close times;
- participant capacity;
- entry fee in USD;
- prize structure in USD;
- status;
- registration requirements;
- game-specific fields;
- settlement rules;
- result-processing configuration.

Core user records must not be polluted with competition-specific game identifiers.

## 9. Registration

Registration is competition-specific.

When a player registers, the system captures:

- player identity;
- competition identity;
- registration timestamp;
- status;
- required dynamic field values;
- payment/entry-fee relationship;
- game-specific identifiers required by that competition.

A player may therefore provide different game identifiers for different competitions.

## 10. Player application

The player dashboard includes:

- Dashboard;
- Games/Tournaments;
- Wallet;
- Results & History;
- Notifications;
- Tutorials;
- Support;
- Account/Settings.

The dashboard should show useful financial and competition summaries without exposing administrative controls.

## 11. Admin application

The admin dashboard includes:

- Overview;
- Users;
- Competitions;
- Registrations;
- Settlements;
- Payments;
- Wallet/Transactions;
- Withdrawals;
- Financial Reconciliation;
- Notifications;
- Support.

Administrative actions must be role-protected and audited.

## 12. Settlement

Settlement is controlled and explicit.

The intended flow is:

1. competition/match evidence is recorded;
2. result data may be extracted by AI or entered by an administrator;
3. structured result data is imported;
4. BG Arena validates the result;
5. the admin reviews detected participants, results, kills/finishes or other game-specific metrics as applicable;
6. the platform calculates winnings according to the competition's settlement configuration;
7. the admin explicitly confirms settlement;
8. the system performs an atomic financial settlement;
9. winners receive USD ledger credits;
10. competition settlement is marked complete;
11. duplicate settlement is prevented.

Game-specific settlement logic must live in configurable settlement rules/adapters rather than the generic competition table.

## 13. AI result extraction

AI is an extraction aid, not a financial authority.

AI output must be treated as untrusted structured input. BG Arena validates:

- JSON syntax;
- schema;
- competition ID;
- registration/player identities;
- numeric ranges;
- duplicate players/results;
- required fields;
- settlement-rule compatibility;
- amount calculations.

No AI output may directly credit money.

## 14. Withdrawals

Player withdrawal flow:

1. player requests withdrawal;
2. player selects/configures the supported payout method and destination information;
3. BG Arena validates the request;
4. available funds are checked;
5. requested funds are reserved;
6. a unique withdrawal ID is created;
7. status becomes PENDING;
8. admin notification is generated;
9. email notification may be generated;
10. payout JSON/instruction data is generated;
11. administrator transfers/imports the instruction into the private payout application;
12. payout application executes payout externally;
13. payout outcome is reconciled back into BG Arena through the defined administrative workflow;
14. withdrawal is marked completed, failed, rejected, or otherwise finalized;
15. reserved funds are finalized or released according to the outcome.

BG Arena must not contain payout-provider secret credentials.

## 15. Financial reconciliation

Admin reconciliation must calculate the amount BG Arena owes to players and provide sufficient information to compare internal liabilities against actual external funds/payment records.

Reconciliation should support grouping by:

- payment method;
- transaction type;
- withdrawal status;
- currency/provider where relevant;
- date range.

## 16. Notifications

Important events can generate in-app and/or email notifications, including:

- registration confirmation;
- payment confirmation;
- deposit credited;
- competition updates;
- results;
- winnings credited;
- withdrawal requested;
- withdrawal status changes;
- support responses;
- administrative announcements.

## 17. Profile representation

No profile image is required for the basic player account.

The default avatar is:

- the first letter of the user's name;
- capitalized;
- displayed in a circular avatar;
- paired with a deterministic or stored assigned background color.

This reduces storage requirements and keeps the platform lightweight.

## 18. Storage

Do not store large media files directly in PostgreSQL database rows.

Images, videos, audio, and other large objects should use appropriate object storage when the feature actually requires them. Store references/metadata in the database.

## 19. Security and auditability

All money-affecting actions must be auditable.

All privileged operations must be authorized server-side.

All external webhooks must be verified using the provider's supported authentication/signature mechanism where available.

Every external payment event must be idempotent.

## 20. Extensibility

The architecture must allow future additions such as:

- additional games;
- additional countries;
- additional currencies for deposits;
- additional mobile-money providers;
- additional crypto providers;
- additional payout providers in the private payout application;
- additional tournament formats;
- additional result extraction methods.

These additions must not require rewriting the core wallet or user architecture.

## 21. Explicitly removed concepts

The following are not part of the current architecture unless explicitly reintroduced:

- collateral system;
- multi-currency internal wallets;
- permanent PUBG-specific player profile fields;
- direct payout execution by BG Arena;
- phone-number authentication.

## 22. Production philosophy

BG Arena should be built as a small, understandable, auditable system rather than a collection of disconnected features. Financial correctness and security take priority over rapid UI completion.
