# WITHDRAWALS AND EXTERNAL PAYOUTS

## Separation principle

BG Arena manages the player's financial obligation. It does not execute the payout-provider API call.

## Request flow

Player -> withdrawal validation -> funds reservation -> PENDING withdrawal -> admin notification -> payout instruction generation -> manual transfer to private payout application -> external payout -> outcome/reconciliation -> final BG Arena status.

## Validation

Before reservation, validate:

- authenticated account;
- account status;
- amount > 0;
- sufficient available balance;
- supported payout method;
- required destination information;
- any platform eligibility/risk rules;
- no conflicting withdrawal state.

## Reservation

Reservation is an atomic financial operation. Once reserved, those funds cannot satisfy another withdrawal or competition entry.

## Payout JSON

The generated instruction should contain only what the private payout application needs, for example:

- withdrawal ID;
- player/internal beneficiary reference;
- payout method;
- destination details required for execution;
- USD amount;
- any required converted payout amount/currency if externally determined;
- creation timestamp;
- integrity/reference information.

Do not include BG Arena secrets or provider credentials.

## Private payout application

The independent payout app has its own database, authentication, credentials, provider integrations and execution logs. It must be treated as a separate trust boundary.

## Completion

The external payout result must be associated with the withdrawal ID. A payout marked completed must not be completed twice.

## Failure

If payout fails before funds are consumed, the reservation may be released according to the defined state machine. If the provider reports an ambiguous state, do not automatically release funds until reconciliation establishes the outcome.

## Privacy

Payout destination information is sensitive. Store only what is required and restrict access by role.
