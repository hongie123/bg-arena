# BG ARENA — PRODUCT REQUIREMENTS

## 1. Users and roles

### Player
A player can create an account, authenticate by email, maintain a profile, browse competitions, register, pay entry fees, participate, view results, receive winnings, deposit funds, request withdrawals, receive notifications, and contact support.

### Admin
An authorized administrator manages users, competitions, registrations, payments, settlements, withdrawals, reconciliation, notifications, and support.

### System/service roles
Server-side services process authenticated provider events, exchange-rate lookups, settlement operations, notifications, and other controlled automation.

## 2. Account requirements

Required account concepts:

- unique user ID;
- email;
- display/name information;
- phone number as profile/contact data when collected;
- account status;
- role;
- created/updated timestamps;
- last authentication information where appropriate.

Phone number must never become the authentication mechanism unless the product specification is explicitly changed.

## 3. Player requirements

The player must be able to:

1. register with email;
2. sign in/out;
3. recover access through the configured email-authentication flow;
4. complete profile information;
5. browse available competitions;
6. inspect competition rules, schedule, entry fee, capacity and prize structure;
7. register for eligible competitions;
8. provide competition-specific registration data;
9. pay an entry fee from available wallet funds or through the defined deposit flow;
10. view wallet balance and transaction history;
11. deposit using available payment methods;
12. see deposited source currency/amount and resulting USD credit in history;
13. view competition results;
14. view winnings;
15. request withdrawals;
16. see withdrawal status;
17. receive notifications;
18. use tutorials and support;
19. manage account settings.

## 4. Competition requirements

Administrators must be able to create competitions with configurable:

- game;
- title;
- description;
- banner/visual references if later implemented;
- format;
- rules;
- registration window;
- event schedule;
- participant limit;
- entry fee USD;
- prize structure USD;
- status;
- registration fields;
- settlement configuration.

Competition statuses should be explicit and validated, for example:

DRAFT -> PUBLISHED -> REGISTRATION_OPEN -> REGISTRATION_CLOSED -> IN_PROGRESS -> RESULT_PENDING -> SETTLEMENT_PENDING -> SETTLED -> COMPLETED/CANCELLED.

Transitions must be controlled and audited.

## 5. Registration requirements

Registration must validate:

- authenticated player;
- competition status;
- registration window;
- capacity;
- duplicate registration;
- required dynamic fields;
- field formats;
- eligibility rules;
- entry-fee requirements.

A registration must have a unique player+competition relationship unless the competition explicitly permits multiple entries.

## 6. Wallet requirements

Wallet display must make USD clear.

The player should see:

- available USD;
- reserved USD where relevant;
- transaction history;
- deposit action;
- withdrawal action.

The client cannot directly alter wallet balances.

## 7. Payment requirements

Payment screens must clearly identify:

- method;
- amount in source denomination;
- estimated/confirmed USD equivalent where applicable;
- fees where applicable;
- payment status;
- provider/reference information when safe to expose.

Cameroon-only payment methods must be labelled appropriately.

## 8. Admin requirements

Administrators need operational visibility into:

- total users;
- active competitions;
- registrations;
- deposits;
- wallet liabilities;
- pending withdrawals;
- pending settlements;
- failed payment events;
- reconciliation differences;
- support workload.

## 9. Audit requirements

For sensitive actions, retain:

- actor;
- action;
- target entity;
- before/after state where appropriate;
- timestamp;
- reason/comment where required;
- correlation/reference ID.

## 10. UX requirements

The application must provide loading, empty, success, error and permission-denied states.

Financial actions must display confirmation before irreversible or money-affecting operations.

Do not hide important fees, conversion information, withdrawal status, or transaction state.
